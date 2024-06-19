---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "ReentrantLock"
date: "2021-05-07"
categories: 
  - "Backend Thread"
---


## ReentrantLock介绍
{% include figure.liquid loading="eager" path="assets/img/2021/05/image-5d45f127d8744f59b9d40da342c88458.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}


> ReentrantLock是基于Lock实现的可重入锁，所有Lock都死基于AQS实现，AQS和Condition各自维护不同的对象，在使用Lock和Condition时，其实就是两个队列的互相移动。它所提供的共享锁、互斥锁都是基于对state的操作。而它的可重入以为实现了同步器Sync，再Sync的两个实现类中，包括了公平锁和非公平锁。

## 源码

### 初始化
```java
ReentrantLock lock = new ReentrantLock(true); // true:公平锁 lock.lock();
try {
// todo
} finally {
lock.unlock(); }
}
```
- 初始化构造函数入参，选择是否为初始化公平锁。
- 其实一般情况下并不需要公平锁，除非你的场景中需要保证顺序性。
- 使用 ReentrantLock 切记需要在 finally 中关闭，lock.unlock()。


### 公平锁、非公平锁、默认
```java
public ReentrantLock(boolean fair) {
sync = fair ? new FairSync() : new NonfairSync();
}
```

**默认是非公平锁**
- 构造函数中选择公平锁(FairSync)、非公平锁(NonfairSync)。

### 公平锁与非公平锁的区别
**hasQueuedPredecessors方法**
```java
static final class FairSync extends Sync {
	protected final boolean tryAcquire(int acquires) { 
		final Thread current = Thread.currentThread(); 
		int c = getState();
		if (c==0){
			if (!hasQueuedPredecessors() && 			 
                            compareAndSetState(0, acquires)) 
				{ setExclusiveOwnerThread(current); 
					return true;
				} 
	}
```
> 公平锁和非公平锁，主要是在方法 tryAcquire 中，是否 有 !hasQueuedPredecessors() 判断。

#### hasQueuedPredecessors
```java
public final boolean hasQueuedPredecessors() {
	Node t = tail; // Read fields in reverse initialization order Node h = head;
	Node s;
	return h != t &&
		((s = h.next) == null || 
		s.thread != Thread.currentThread());
}
```
- 在这个判断中主要就是看当前线程是不是同步队列的首位，是:true、否:false
- 这部分就涉及到了公平锁的实现，CLH(Craig，Landin andHagersten)。三个作者
  的首字母组合


## 公平锁
公平锁就像是马路边上的卫生间，上厕所需要排队。当然如果有人不排队，那么 就是非公平锁了，比如领导要先上。

### CLH
**是一种基于单向链表的高性能、公平的自旋锁**。AQS中的队列是CLH变体的虚拟双向队列(FIFO)，AQS是通过将每条请求共享资源的线程封装成一个节点来实现锁的分配。

## 总结
- ReentrantLock 使用起来更加灵活，可操作性也更大，但一定要在 finally 中释放 锁，目的是保证在获取锁之后，最终能够被释放。同时不要将获取锁的过程写在 try 里面。
- 公平锁的实现依据不同场景和 SMP、NUMA 的使用，会有不同的优劣效果。在实 际的使用中一般默认会选择非公平锁，即使是自旋也是耗费性能的，一般会用在较少等待的线程中，避免自旋时过长。
















