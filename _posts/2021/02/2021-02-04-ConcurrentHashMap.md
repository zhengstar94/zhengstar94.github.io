---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "ConcurrentHashMap"
date: "2021-02-04"
categories: 
  - "Backend Collection"
---


# JDK1.7
## 存储结构
ConcurrentHashMap由很多个segment组成，每一个segment是一个类似于HashMap的结构，所以一个HashMap的内部可以进行扩容，但是segment个数一旦初始化之后就不能饿改变，默认segment的个数为16个，相当于ConcurrentHashMap默认支持最多16个线程的并发

## PUT方法
1. 计算put的key的位置吗，获取指定位置的segment
2. 如果segment为空，则初始化这个segment
  1. 检查计算的segment是否为null
  2. 如果为null继续初始化，使用segment[0]的容量和负载因子创建一个HashEntry数组
  3. 再次检查计算得到的指定位置的segment是否为null
  4. 使用创建的HashEntry数组初始化这个segment
  5. 自旋判断计算得到的指定位置的segmant是否为null，使用cas在这个位置赋值为segment
3. segment插入key和value
4. 计算put数据需要放入的index位置，然后获取这个位置上的HashEntry
5. 遍历put新元素，由于获取的HashEntry可能是一个空元素，也可能是已经存在的链表
  1. 如果这个位置的Entry不存在
        1. 如果当前容量大于扩容的阈值，小于最大容量，进行扩容
            2. 直接头插法
  2. 如果这个位置的HashEntry存在
        1. 判断链表当前元素key和hash是否要与put的key和hash一致，一致则直接替换
            2. 不一致，获取链表下一个节点，直到发现相同进行值的替换，或者链表没有相同
      1. 如果当前容量大于扩容的阈值，小于最大容量，进行扩容
      2. 直接链表头插法
6. 如果插入的位置之前存在相同，替换后返回旧值，否则返回null


## GET
1. 计算得到key的存放位置
2. 遍历指定位置查找相同的key的value值


# JDK1.8
## 存储结构
不再是segment数组+HashEntry数组+链表，而是变成Node数组+链表/红黑树，当冲突链表达到一定长度，链表转为红黑树。synchronized+CAS+HashEntry+红黑树


## PUT
**采用CAS+synchronized实现并发插入或更新操作**

1. 根据key计算出hashcode
2. 判断当前是否需要初始化
3. 即为当前key定位出的node，如果为空表示当前位置可以写入数据，利用cas尝试写入，失败则自旋保证成功
4. 如果当前位置的hashcode==moved==-1，则需要扩容
5. 如果都不满足，利用synchronized锁写入数据
6. 如果数量大于阈值则需要转为红黑树


## GET
1. 根据hash值计算位置
2. 查找到指定位置，如果头节点就是要查找的，直接返回value
3. 如果头节点hash值小于0，说明正在扩容或者是红黑树，直接遍历查找
4. 如果是链表，遍历查找


## 总结
java7中的ConcurrentHashMap使用的分段锁，也就是每一个segment上同时只有一个线程可以操作，每一个segment都是一个类似HashMap数组的结构，它可以扩容，冲突则转为链表，但是segment的个数一旦初始化就不能改变

java8的ConcurrentHashMap使用的synchronized锁加cas机制，结构变为Node数组+链表/红黑树，Node是一个类似于一个HashEntry的结构，它的冲突达到一定大小会转为红黑树，冲突小于一定数量又回退回链表

> ##### 为什么线程安全呢？
>
>就是因为在1.7版本它的每个方法都会加锁，put采用自旋锁不会使线程阻塞（操作状态切换会消耗性能），从而性能比hashtable好很多。而且它的每个HashEntry[i]都是被volatile修饰，可以保证线程操作的可见性，即每次不会脏读，即使其他线程修改了值，都会强制刷新到本地内存。
{: .block-tip }


> ##### 那为什么并发度高呢？
>
>因为是对单个segment[i]进行加锁，意思就是segment如果有16个，那么可以同时有16个线程修改而且还是线程安全的。相对于Hashtable的锁，是锁定整个Hashtable对象，那么多个线程访问就需要被阻塞。
{: .block-tip }


 
