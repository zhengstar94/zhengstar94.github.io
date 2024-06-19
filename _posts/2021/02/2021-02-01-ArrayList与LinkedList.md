---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "ArrayList与LinkedList"
date: "2021-02-01"
categories: 
  - "Backend Collection"
---

## ArrayList

### 简介
底层由数组，初始化数据为0，容量动态增长，在add的时候容量默认为10，每次扩容为原来的1.5倍，把原来的数组复制到新的数组，再把原来的地址指向新的地址

### 特性
查询速度比较快，增删效率比较低，增删都需要进行数组复制以及移动

### 扩容机制
JDK1.8中，如果通过无参构造的话，初始数组容量为0，当真正对数组进行添加的时候，才开始分配容量，默认为10；当容量不足，则以1.5倍扩容；

### add操作
执行add方法，先判断ArrayList当前容量是否满足size+1的容量；
在判断是否满足size+1容量时，先判断ArrayList是否为空；如果为空，初始化ArrayList为10，再判断初始容量是否满足最低要求；如果不为空，则直接判断初始容量是否满足最低要求；若满足最低容量要求，直接添加；若不满足，则先扩容，再添加；

## LinkedList

### 基础
底层带有头节点和尾节点的双向链表，所以包含头插法与尾插法，所以适合增删的场景，由于是链表查询，需要一个个按顺序遍历对比，查询速度较慢


## 两者的对比
1. ArrayList基于动态数组，LinkedList基于链表
2. 对于随机访问的get与set，ArrayList优于LinkedList，因为LinkedList需要移动指针
3. 对于增删，如果数据比较大，LinkedList优于ArrayList，因为ArrayList需要移动数据
4. 都不保证线程安全
5. ArrayList在空间的浪费主要体现在list列表的结尾都会预留一定容量的空间，LinkedList空间的花费则体现在每个数据都需要消耗比ArrayList更多的空间（需要存放直接前驱与后继）

> 当操作是在一列数据的后面添加数据而不是在前面或中间,并且需要随机地访问其中的元素时,使用ArrayList会有更好的性能；当操作是在一列数据的前面或中间添加或删除数据,并且按照顺序访问其中的元素时,就应该使用LinkedList了


****
## 常见面试题

### 1. 数组越界
创建ArrayList后直接调用了add(int index, E element)的方法，结果返回了一个索引越界的异常

```java
public static void main(String[] args) { 
   List<String> list = new ArrayList<String>(10); 
   list.add(2, "1");         
       System.out.println(list.get(0));
}
```
> Exception in thread "main" java.lang.IndexOutOfBoundsException: Index: 2, Size: 0 at java.util.ArrayList.rangeCheckForAdd(ArrayList.java:665)
> at java.util.ArrayList.add(ArrayList.java:477)
> at org.itstack.interview.test.ApiTest.main(ApiTest.java:13)


**解答：**
ArrayList的一个方法

```java
/**
 * A version of rangeCheck used by add and addAll. 
 */
 private void rangeCheckForAdd(int index) {
    if (index > size || index < 0)
        throw new  IndexOutOfBoundsException(outOfBoundsMsg(index));
 }
```
rangeCheckForAdd函数中的判断是index > size || index < 0 就会抛出下标越界异常，再来看看size的定义

```java
/**
 * The size of the ArrayList (the number of elements it contains). * * @serial
 */
private int size;

/**
 * Returns the number of elements in this list. * * @return the number of elements in this list
 */
 public int size() {
    return size;
}
```

> 从size定义的注释和size()函数就能很明显的看出，size代表的是ArrayList中有多少个元素。只有当调用ArrayList的add()和remove()方法时，才会对size进行操作(size++, --size)，所以当我们新建ArrayList时，size=0，此时调用add(E element)直接添加元素是没有问题的，而此时调用add(int index, E element)，若index > 0就会抛出下标越界异常



## 补充知识
### 面试常问：快速失败 (fail-fast) 和安全失败 (fail-safe) 的区别是什么？

> -快速失败：快速失败的迭代器会抛出ConcurrentModificationException异常。当迭代一个集合的时候，如果有另外一个线程在修改这个集合，就会抛出ConcurrentModification异常。不能在多线程下发生并发修改。
> 安全失败：java.util.concurrent包下面的所有的类都是安全失败的。Iterator的安全失败是基于对底层集合做**拷贝**，因此，它不受源集合上修改的影响。采用安全失败机制的集合容器，在遍历时不是直接在集合内容上访问的，而是先复制原有集合内容，在拷贝的集合上进行遍历。
