---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "AbstractQueuedSynchronizer"
date: "2021-05-08"
categories: 
  - "Backend Thread"
---


# AQS是什么(AbstractQueuedSynchronizer)
- 用于构建锁或其他同步器组建的重量级基础框架及整个JUC体系的基石，**通过内置的CLH(FIFO)队列的变种来完成资源获取线程的排队工作，将每条要抢占资源的线程封装成一个Node节点来实现锁的分配，一个int类变量表示持有锁的状态，通过CAS完成对state值的修改**。

> FIFO是一CLH单向链表变体的虚拟双向队列

是可以给我们实现锁的一个框架，内部实现的关键就是维护了一个先进先出的队列以及state状态变量，先进先出队列存储的载体叫做Node节点，该节点标识着当前的状态值，是独占还是共享模式以及它的前驱和后继节点等信息。

**总体流程：会把需要等待的线程以Node的形式放到这个先进先出的队列上，state变量则表示为当前锁的状态**

- AQS框架下的类
   **(ReentrantLock | CountDownLatch | ReentrantReadWriteLock | Semaphore)**

{% include figure.liquid loading="eager" path="assets/img/2021/05/image-6a355ca6ddeb49ce9a97e82a4e0c7f7b.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

> 以上的类都是通过内部类Sync继承AbstractQueuedSynchronizer 实现的

- 加锁会导致阻塞，有阻塞就需要排队，排队必然需要队列

- 如果共享资源被占用，就需要一定的阻塞等待唤醒机制来保证锁分配。这个机制主要用的FIFO，将暂时获取不到的线程加入到队列中，这个队列就是AQS的抽象表现，共享资源封装为队列的结点，通过CAS、自旋以及LockSuport.park()方式，维护state变量的状态，使并发达到同步的效果。

{% include figure.liquid loading="eager" path="assets/img/2021/05/image-ff25afe213d347338a4b2b2a8ca5e0bc.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

# 加锁与解锁的流程
{% include figure.liquid loading="eager" path="assets/img/2021/05/image-6495a2af3d754f8ba20dc954e517f838.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}


# AQS内部体系架构
## 内部架构图

{% include figure.liquid loading="eager" path="assets/img/2021/05/image-0fe6a29bc8234d6f95e7cf43f097bb33.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

{% include figure.liquid loading="eager" path="assets/img/2021/05/image-261e35cb017b4f2f8adbf4736d59c1e4.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

## 详解AQS内部代码

{% include figure.liquid loading="eager" path="assets/img/2021/05/image-52662b285b8a4a0690f34a16bf9a83bf.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}


## CLH队列，称为一个双向队列

{% include figure.liquid loading="eager" path="assets/img/2021/05/image-7625d62619604736830b809862c63fd4.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

## 内部结构(Node类详解)
{% include figure.liquid loading="eager" path="assets/img/2021/05/image-e312ce5103554cf0a502e3d93b1a20a8.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

{% include figure.liquid loading="eager" path="assets/img/2021/05/image-2ccd6ab7f9cc4ec6913016d0bfc6781f.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

## AQS同步队列的基本结构
{% include figure.liquid loading="eager" path="assets/img/2021/05/image-c31a37769b5345ae80c32c6bd1ee6bb2.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

# ReentrantLock解读AQS

**业务图：**

{% include figure.liquid loading="eager" path="assets/img/2021/05/image-d57b1f5c73f144129e56bb0cbf255185.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

**业务代码：**

```java

public class AQSDemo {
    public static void main(String[] args) {
        ReentrantLock lock = new ReentrantLock();
        //带入一个银行办理业务的案例来模拟我们的AQS如何进行线程的管理和通知唤醒机制
        //3个线程模拟3个来银行网点,受理窗口办理业务的顾客
        //A顾客就是第一个顾客,此时受理窗口没有任何人,A可以直接去办理
        new Thread(() -> {
                lock.lock();
                try{
                    System.out.println("-----A thread come in");

                    try { TimeUnit.MINUTES.sleep(20); }catch (Exception e) {e.printStackTrace();}
                }finally {
                    lock.unlock();
                }
        },"A").start();

        //第二个顾客,第二个线程---》由于受理业务的窗口只有一个(只能一个线程持有锁),此时B只能等待,
        //进入候客区
        new Thread(() -> {
            lock.lock();
            try{
                System.out.println("-----B thread come in");
            }finally {
                lock.unlock();
            }
        },"B").start();

        //第三个顾客,第三个线程---》由于受理业务的窗口只有一个(只能一个线程持有锁),此时C只能等待,
        //进入候客区
        new Thread(() -> {
            lock.lock();
            try{
                System.out.println("-----C thread come in");
            }finally {
                lock.unlock();
            }
        },"C").start();
    }
}


```

## lock方法看公平锁与非公平锁

{% include figure.liquid loading="eager" path="assets/img/2021/05/image-e9f7bebfe5564c519b0457f843f561dc.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

**默认创建的是非公平锁**

公平锁与非公平锁的差异：

{% include figure.liquid loading="eager" path="assets/img/2021/05/image-37af8dbecbf14862b06ce26893a78719.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

> 可以看出公平锁与非公平锁lock()方法唯一区别就是公平锁在获取同步状态时多了一个限制条件:hasQueuedPredecessoros()<br>
> hasQueuedPredecessoros是公平锁加锁时判断等待队列是否存在有效节点的方法

{% include figure.liquid loading="eager" path="assets/img/2021/05/image-47f363041cc2418680acacfad8e121e8.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

## lock()

### 1. lock.lock()源码

{% include figure.liquid loading="eager" path="assets/img/2021/05/image-db506d4c3b4e479492f0a47daed3f0bc.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

### 2. acquire()源码和3大流程走向

{% include figure.liquid loading="eager" path="assets/img/2021/05/image-8b10d153133b4df6987fa383d8d843e0.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

#### 1. tryAcquire(arg)
##### 1. 本次走非公平锁方向
{% include figure.liquid loading="eager" path="assets/img/2021/05/image-48709f84a03a4961afbec3765c6552ef.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}
##### 2.  nonfairTryAcquire(acquires)
> return false(继续推进条件,走下一步方法addWaiter)<br>
> return true(结束)

{% include figure.liquid loading="eager" path="assets/img/2021/05/image-78c6d1eb178d44b4ba96eec02d8dd922.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

#### addWaiter(Node.EXCLUSIVE)
##### 1. addWaiter
{% include figure.liquid loading="eager" path="assets/img/2021/05/image-c93d76fbc2a94dd6b1ebdbc6d562a46d.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

**图解：**
{% include figure.liquid loading="eager" path="assets/img/2021/05/image-81b52139e662476fb6246935220ca1df.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

##### 2. enq(node)
{% include figure.liquid loading="eager" path="assets/img/2021/05/image-c92a66522c944845a766b7377997d4f1.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

##### 3. B、C线程排好队效果如下图
{% include figure.liquid loading="eager" path="assets/img/2021/05/image-b2584cb3b1fc4aea83a925f642b9c63c.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}


#### acquireQueued(addWaiter(Node.EXCLUSIVE), arg)
##### 1. acquireQueued会调用如下方法
(会调用如下方法:shouldParkAterFailedAcquire和parkAndCheckInterrupt | setHead(node) )

##### 2. shouldParkAterFailedAcquire
{% include figure.liquid loading="eager" path="assets/img/2021/05/image-beb4ef1e6d0145928bd41dbdf0a90fe1.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

##### 3. parkAndCheckInterrupt
{% include figure.liquid loading="eager" path="assets/img/2021/05/image-89efd434733240debb171e9bf007a06f.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}
> LockSupport.park() 挂起线程

##### 4. 当我们执行下图中的③表示线程B或者C已经获取了permit了

{% include figure.liquid loading="eager" path="assets/img/2021/05/image-e31bace7fb0c4c8d96b5289fc91c99c7.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

##### 5.setHead()
代码执行完成之后会出现下图所示

{% include figure.liquid loading="eager" path="assets/img/2021/05/image-e9b7590464af45a6bc2967d84db68be6.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}
{% include figure.liquid loading="eager" path="assets/img/2021/05/image-13eed9af6e334814a42ab53f41ceb7ea.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

> 相当于队列中原本傀儡节点由于指向为空，就被GC，B线程替代变为傀儡节点

## unlock()获取permit
### 1. release | tryRelease | unparkSuccessor(h);
{% include figure.liquid loading="eager" path="assets/img/2021/05/image-4d7bac7acd984efd8e76f1a25625b7f3.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

### 2. tryRelease()
{% include figure.liquid loading="eager" path="assets/img/2021/05/image-646b202997154cb2a1082207a55aa271.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

### 3. unparkSuccessor()
{% include figure.liquid loading="eager" path="assets/img/2021/05/image-fa8a83adeca04d4e9a95302c4c6f5cb3.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

# AQS源码总结
1. 业务场景,比如说我们有三个线程A、B、C去银行办理业务了,A线程最先抢到执行权开始办理业务,那么B、C两个线程就在CLH队列里面排队如图所示,注意傀儡结点和B结点的状态都会改为-1

{% include figure.liquid loading="eager" path="assets/img/2021/05/image-dd7745c833f3400ab010440607438975.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

2. 当A线程办理好业务,离开的时候,会把傀儡结点的waitStatus从-1改为0 | 将status从1改为0,将当前线程置为null
3. 这个时候如果B上位,首先将status从0改为1(表示占用),把thread置为线程B | 会执行如下图的①②③④,会触发GC,然后就把第一个灰色的傀儡结点给清除掉了,这个时候原来的B结点重新成为傀儡结点。
   {% include figure.liquid loading="eager" path="assets/img/2021/05/image-dffd2223a4014bb99cc61e3f68a94f0d.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}



# 实现AQS同步类
1. Sync类继承```AbstractQueuedSynchronizer```，并重写方法：```tryAcquire```、```tryRelease```、```isHeldEXclusively```；
2. 这3个方法必须重写，如果不重写在使用的时候会抛异常```UnsupportedOperationException```
3. 重写过程也比较简单，主要是使用AQS的CAS方法。以预期值为0，更新值为1，写入成功则获取锁成功。其实这个过程就是对state使用unsafe本地方法，传递偏移量stateOffset等参数，进行值交换操作。
4. 最后提供lock与unlock两个方法，实际的类中会实现Lock接口中的相应方法，这里为了简化直接自定义这样两个方法


****


# AQS核心源码分析

## 公平锁的实现原理
> AQS有个队列，队首是Runnable状态的线程，后面都是Waiting状态的线程，公平锁是在cas修改state之前先判断队列里是否有Waiting状态的线程，如果有就将当前线程加到队尾

## 1. 获取锁流程图
{% include figure.liquid loading="eager" path="assets/img/2021/05/image-cac25a07388449d49092ad62e8f532d1.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

## 2. lock
{% include figure.liquid loading="eager" path="assets/img/2021/05/image-e152788fb84e4077b97e21e7289a5a4d.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

ReentrantLock实现了非公平锁，在调用```lock.lock()```时候,会有不同实现类
- 非公平锁：会直接使用CAS抢占，修改变量state的值，如果成功则直接把自己的线程设置到exclusieOwnerThread(排他线程)，也就是获得锁成功
- 公平锁：不会进行抢占，规矩排队

## 3. acquire
{% include figure.liquid loading="eager" path="assets/img/2021/05/image-e4547a972ffb4cc5a056929278dd6ef0.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

这个代码包含四个方法的调用：

1. tryAcquire：分别由继承AQS的公平锁和非公平锁实现；
2. addWaiter：AQS的私有方法，主要用途是tryAcquire返回false后，也就是获取锁失败后，把当前请求锁的线程添加到队列中，并返回Node节点；
3. acquireQueued：负责把addWaiter返回的Node节点添加到队列的后面，并会执行获取锁操作以及判断是否把当前线程挂起
4. selfInterrupt：主要是执行完acquire之前自己执行中断操作

## 4. tryAcquire
{% include figure.liquid loading="eager" path="assets/img/2021/05/image-389a0a7a839d41529409e99cb4fba1e1.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

这块代码包含两部分：
1. 如果```c==0```，锁没有被占用，尝试用CAS方式获取锁，返回true
2. 如果```current == getExclusiveOwnerThread()```，也就是当前线程持有锁，则需要调用```setState```进行锁重入操作。setState不需要枷锁，因为是在自己的当前线程下(可重入锁)
3. 如果两种都不满足返回false

## 5. addWaiter
```java
private Node addWaiter(Node mode) {
	Node node = new Node(Thread.currentThread(), mode); 
	Node pred = tail;
	// 如果队列不为空, 使用 CAS 方式将当前节点设为尾节点
	if (pred != null) 
	{
		node.prev = pred;
		if (compareAndSetTail(pred, node))
 		{
			pred.next = node;
			return node; 
		}
	}
	// 队列为空、CAS失败，将节点插入队列 
	enq(node);
	return node;
}
```
- 当前执行方法```addWaiter```，那么就是```!tryAcquire =true```，也就是tryAcquire获取锁失败
- 把当前节点封装到Node节点中，加入到FIFO队列中。因为先进先出，所以后来的队列加入到队尾
- ```compareAndSetTail```不一定成功，并发场景下，可能会出现操作失败，如果操作失败需要调用```enq```方法，该方法会自旋操作，把节点入队列

### enq
{% include figure.liquid loading="eager" path="assets/img/2021/05/image-0b497f9b3ce24074865de172d08fe965.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

- 自旋for循环+CAS入队列
- 队列为空会常见一个节点，把尾节点志向头节点，然后继续循环
- 第二次循环，会把当前线程的节点添加到队尾。head节点是一个无用节点

**注意，从尾节点逆向遍历**

## 6. acquireQueued
{% include figure.liquid loading="eager" path="assets/img/2021/05/image-f7fe722eff04456caea4dc8634647b95.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

当获取锁流程到这一步，说明节点已经加入队列完成。接下来就是让方法再次尝试获取锁，如果获取锁失败会判断是否把线程挂起。

### setHead
```java
private void setHead(Node node) 
{ 
	head = node;
	node.thread = null;
	node.prev = null; 
}
```
> Head是一个虚节点，如果当前节点的前驱节点是Head节点，说明此时Node节点排在队列最前面，可以尝试获取锁。获取到锁之后设置Head节点，就是一个出队的过程，原来设置的节点设置Null方便GC。

### shouldParkAfterFailedAcquire
{% include figure.liquid loading="eager" path="assets/img/2021/05/image-e2068659d47e4a84b81b8f3605634c10.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

1. CANCELLED，取消排队，放弃获取锁。
2. SIGNAL，标识当前节点的下一个节点状态已经被挂起，其实就是当前线程执行完毕后，需要额外执行唤醒后继节点操作。


   那么，以上这段代码主要的执行内容包括:

1. 如果前一个节点状态是 SIGNAL，则返回 true。安心睡觉等着被叫醒
2. 如果前一个节点状态是 CANCELLED，就是它放弃了，则继续向前寻找其他节 点。
3. 最后如果什么都没找到，就给前一个节点设置个闹钟 SIGNAL，等着被通知。


### parkAndCheckInterrupt

```java
private final boolean parkAndCheckInterrupt() 
{
	//park方法真正加锁
	LockSupport.park(this);
	return Thread.interrupted(); 
}
```
- 方法```shouldParkAfterFailedAcquire```返回false，执行parkAndCheckInterrupt方法
- ```LockSupport.park(this);```挂起线程操作
- ``` Thread.interrupted(); ```检查线程中断标识









