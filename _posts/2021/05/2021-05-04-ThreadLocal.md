---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "ThreadLocal"
date: "2021-05-04"
categories: 
  - "Backend Thread"
---

## 简介
ThreadLocal主要解决让每个线程绑定自己的值，可以将ThreadLocal比喻成放数据的盒子，盒子中可以存储每个线程的私有数据；

如果创建了一个ThreadLocal遍历，那么访问这个变量的每个线程都会有这个变量的本地副本，他们可以使用get或set方法来获取或修改当前线程所存的副本的值，从而避免了线程安全的问题。

## 数据结构
{% include figure.liquid loading="eager" path="assets/img/2021/05/image-b23c7d48a37b4e6a9b025b6b8df34d6d.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

1. 它是一个数组
2. 数据元素采用哈希散列方式进行存储，这里的散列使用的是斐波那契散列
3. 这里不同于HashMap的数据结构，发生哈希碰撞不会存在链表或红黑树，而是使用拉链法进行存储。

## 实现原理
底层是通过ThreadLocalMap实现的，每个Thread对象中都存在一个ThreadLocalMap，Map的key为ThreadLocal对象，value为需要缓存的值

## 内存泄漏问题
ThreadLocalMap中的key为ThreadLocal的弱引用，但是value却是强引用。如果ThreadLocal没有被外部强引用的情况下，在垃圾回收的时候，key会被清除但是value不会被清除掉，所以使得ThreadLocalMap会存在key为null的entry。如果不做任何措施，value会无法被GC回收，这个时候可能会产生内存泄漏问题，所以使用完之后需要我们手动调用ThreadLocal的remove()方法。

- 弱引用介绍
> 垃圾回收时会直接回收掉，不管当前内存空间足够与否，都会回收它的内存


- 强引用
> 也就是new出来的对象，当内存不足的时候，JVM宁愿出现OOM错误，也需要保存这个对象，并且永远不会被GC回收，当然可以手动对对象进行弱化使其可以被回收
```object = null;```


