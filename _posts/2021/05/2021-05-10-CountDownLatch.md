---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "CountDownLatch"
date: "2021-05-10"
categories: 
  - "Backend Thread"
---



## 介绍
CountDownLatch 一个同步辅助工具，同样是基于 AQS 实现

## 详解
CountDownLatch中count down 是倒数的意思，latch是门闩的意思。整体可以理解为倒数的门闩。
在构造CountDownLatch需要传入一个整数n，在这个整数‘倒数’到0之前，主线程需要在门口等待，而这个“倒数”过程则是由各个执行线程驱动的，每个线程执行完一个任务“倒数”一次。

> 总结来说，CountDownLatch的作用就是等待其他的线程都执行完任务，必要时可以对各个任务的执行结果进行汇总，然后主线程才继续往下执行

CountDownLatch主要有两个方法：```countDown()```和```await()```。```countDown()```方法用于使计数器减一，其一般是执行任务的线程调用，```await()```方法则使调用该方法的线程处于等待状态，其一般是主线程调用。

**注意：**
countDown()方法并没有规定一个线程只能调用一次，当同一个线程调用多次countDown()方法时，每次都会使计数器减一；另外，await()方法也并没有规定只能有一个线程执行该方法，如果多个线程同时执行await()方法，那么这几个线程都将处于等待状态，并且以共享模式享有同一个锁。

## 场景
主线程先启动了五个线程，然后主线程进入等待状态，当这五个线程都执行完任务之后主线程才结束了等待。

### 适用场景
- 场景1
  非常适合对于任务进行拆分，使其并行执行，比如某个任务执行2s，其对数据的请求可以分为五个部分，那么就可以将这个任务拆分为5个子任务，分别交由五个线程执行，执行完成之后再由主线程进行汇总，此时，总的执行时间将决定于执行最慢的任务，平均来看，还是大大减少了总的执行时间。

- 场景2
  使用某些外部链接请求数据的时候，比如图片。因为我们使用的图片服务只提供了获取单个图片的功能，而每次获取图片的时间不等，一般都需要1.5s~2s。当我们需要批量获取图片的时候，比如列表页需要展示一系列的图片，如果使用单个线程顺序获取，那么等待时间将会极长，此时我们就可以使用CountDownLatch对获取图片的操作进行拆分，并行的获取图片，这样也就缩短了总的获取时间。

CountDownLatch基于AQS实现的，在AQS中维护了一个volatile类型的整数state，volatile可以保证多线程环境下该变量的修改对每个线程都可见。**创建一个CountDownLatch对象时，所传入的整数n就会赋值给state属性，当countDown()方法调用时，该线程就会尝试对state减一，而调用await()方法时，当前线程就会判断state属性是否为0，如果为0，则继续往下执行，如果不为0，则使当前线程进入等待状态，直到某个线程将state属性置为0，其就会唤醒在await()方法中等待的线程**。


## CyclicBarrier与CountDownLatch区别

- CountDownLatch的参与线程是有不同角色的，有的负责倒计时，有的在等待倒计时变为 0，负责倒计时和等待倒计时的线程都可以有多个，用于不同角色线程间的同步。

- CyclicBarrier的参与线程角色是一样的，用于同一角色线程间的协调一致。

- CountDownLatch是一次性的，而CyclicBarrier是可以重复利用的。
