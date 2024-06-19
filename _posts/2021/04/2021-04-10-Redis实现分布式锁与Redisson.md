---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Redis实现分布式锁与Redisson"
date: "2021-04-10"
categories: 
  - "Backend Redis"
---


## 要点
Redis要实现分布式锁，以下条件应该得到满足

**互斥性**

- 在任意时刻，只有一个客户端能持有锁。

**不能死锁**

- 客户端在持有锁的期间崩溃而没有主动解锁，也能保证后续其他客户端能加锁。

**容错性**

- 只要大部分的Redis节点正常运行，客户端就可以加锁和解锁。

实现
可以直接通过 set key value px milliseconds nx 命令实现加锁， 通过Lua脚本实现解锁。

```bash
//获取锁（unique_value可以是UUID等）
SET resource_name unique_value NX PX  30000

//释放锁（lua脚本中，一定要比较value，防止误解锁）
if redis.call("get",KEYS[1]) == ARGV[1] then
    return redis.call("del",KEYS[1])
else
    return 0
end
```
**代码解释**
- set 命令要用 set key value px milliseconds nx，替代 setnx + expire 需要分两次执行命令的方式，保证了原子性，

- value 要具有唯一性，可以使用UUID.randomUUID().toString()方法生成，用来标识这把锁是属于哪个请求加的，在解锁的时候就可以有依据；

- 释放锁时要验证 value 值，防止误解锁；

- 通过 Lua 脚本来避免 Check And Set 模型的并发问题，因为在释放锁的时候因为涉及到多个Redis操作 （利用了eval命令执行Lua脚本的原子性）；

**加锁代码分析**
1. 首先，set()加入了NX参数，可以保证如果已经又key存在，函数就不会掉用成功，也就是只有一个客户端能够持有锁，满足互斥性；
2. 由于我们对锁设置了过期时间，即使锁的持有者后续发生崩溃没有解锁，锁也会因为到了过期时间自动解锁(key被删除)，不会发生死锁；
3. 因为我们把value设置唯一值标识这把锁属于哪个请求，那么在客户端解锁的时候可以进行校验是否是同一个客户端。

**解锁代码分析**

将LUA代码传到jedis.eval()方法，并使参数Key[1]赋值为lockKey，ARGV[1]赋值为唯一值id。在执行的时候，首先会获取锁对应的value值，检查是否与唯一值id相等，如果相等则解锁(删除key)

**存在的风险**

如果存储锁对应的key节点挂了，可能存在丢失锁的风险，导致多个客户端持有锁，不能实现资源的独享。

1. 客户端A从master获取到锁
2. 在master将锁同步到slave之前，master宕机(Redis的=主从同步是异步的)。主从切换，slave节点被晋升为master节点
3. 客户端B取得了同一个资源被客户端A已经获取到的另外一个锁。导致存在同一时刻存在不止一个线程获取到锁的情况。


## Redisson实现
它实现了可重入锁、公平锁、读写锁等

### 实现原理
- redisson所有指令都通过lua脚本执行，redis支持lua脚本原子性执行

- redisson设置一个key的默认过期时间为30s,如果某个客户端持有一个锁超过了30s怎么办？

redisson中有一个watchdog的概念，翻译过来就是看门狗，它会在你获取锁之后，每隔10秒帮你把key的超时时间设为30s

这样的话，就算一直持有锁也不会出现key过期了，其他线程获取到锁的问题了。

- redisson的“看门狗”逻辑保证了没有死锁发生。

(如果机器宕机了，看门狗也就没了。此时就不会延长key的过期时间，到了30s之后就会自动过期了，其他线程可以获取到锁)

{% include figure.liquid loading="eager" path="assets/img/2021/04/3.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}



### 实现
```java

// 1.构造redisson实现分布式锁必要的Config
Config config = new Config();
config.useSingleServer().setAddress("redis://127.0.0.1:5379").setPassword("123456").setDatabase(0);
// 2.构造RedissonClient
RedissonClient redissonClient = Redisson.create(config);
// 3.获取锁对象实例（无法保证是按线程的顺序获取到）
RLock rLock = redissonClient.getLock(lockKey);
try {
    /**
     * 4.尝试获取锁
     * waitTimeout 尝试获取锁的最大等待时间，超过这个值，则认为获取锁失败
     * leaseTime   锁的持有时间,超过这个时间锁会自动失效（值应设置为大于业务处理的时间，确保在锁有效期内业务能处理完）
     */
    boolean res = rLock.tryLock((long)waitTimeout, (long)leaseTime, TimeUnit.SECONDS);
    if (res) {
        //成功获得锁，在这里处理业务
    }
} catch (Exception e) {
    throw new RuntimeException("aquire lock fail");
}finally{
    //无论如何, 最后都要解锁
    rLock.unlock();
}
```

**加锁流程图**
![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2021/08/image-eaae02e9943941caa0be8361fa907b61.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}


{% include figure.liquid loading="eager" path="assets/img/2021/04/2.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}
> 看门狗原理<br>
> 1. 如果我们指定了锁的超时时间，就发送给redis执行脚本，进行占锁，默认超时就是我们制定的时间，不会自动续期；<br>
> 2. 如果我们未指定锁的超时时间，就使用 lockWatchdogTimeout = 30 * 1000 【看门狗默认时间】<br>
> 3. 只要占锁成功，就会启动一个定时任务【重新给锁设置过期时间，新的过期时间就是看门狗的默认时间】,每隔10秒都会自动再续成30秒；<br>
> 4. 自动续期时间：internalLockLeaseTime 【看门狗时间 30s】 / 3， 10s



**解锁流程图**

{% include figure.liquid loading="eager" path="assets/img/2021/04/1.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

RedissonLock是可重入的，并且考虑了失败重试，可以设置锁的最大等待时间。


















