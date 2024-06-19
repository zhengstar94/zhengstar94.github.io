---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Synchronized"
date: "2021-05-06"
categories: 
  - "Backend Thread"
---


## Synchronized
{% include figure.liquid loading="eager" path="assets/img/2021/05/image-3cef962285294cb79a766a5f01157fc8.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}


### 基础
- 该关键字解决的是多个线程之间访问资源的同步性，被它修饰的方法或者代码库在任意时刻只能有一个线程执行
- 重量级锁，属于悲观锁，会引起线程阻塞
- 早期版本，sychronized属于重量级锁，在1.6之后对sychronized优化比较大，引进锁的升级减少锁操作的开销

### 关键字作用

在并发编程中存在线程安全问题，该synchronized关键字能够避免多线程环境下线程安全问题。

### 底层原理

> synchronized使用的是minitorenter与monitorexit指令，minitorenter指向代码开始的位置，monitorexit指向代码结束位置

在内存中，对象一般由三部分组成，分别是对象头、对象实际数据和对齐填充。重点在于对象头，对象头由几部分组成，我们主要关注对象头```Mark Word```信息，```Mark Word```会记录对象关于锁的信息，每个对象都会有一个与之对应的monitor对象，monitor对象存储着当前持有锁的线程以及等待锁的线程队列。


#### 同步代码块
当执行minitorenter指令，**通过获取对象监视器```monitor```的持有权**
1. 执行monitorenter，会尝试获取对象的锁，如果monitor的计数器为0则表示可以被获取，获取后锁的计数器被+1
2. 当执行monitorexit后，将锁的计数器设为0，表明锁已经被释放。如果获取对象锁失败，那当前就需要阻塞线程等待，直到锁被另一个线程释放为止

#### 同步方法
1. 同步方法时，一旦执行到这个方法，就会先判断是否有标志位，```ACC_SYNCHRONIZED```会去隐式调用monitorenter和monitorexit，总而言之，**依旧是monitor对象的争夺**

{% include figure.liquid loading="eager" path="assets/img/2021/05/image-d6b6cf4c574a48d9a31f7617bded4a7e.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}


### synchronized关键字使用
#### 三种使用方式

1.修饰实例方法(锁是当前实例对象)
```java
synchronized void method(){
//业务方法
}
```
2.修饰静态方法(锁是当前类的class对象)
```java
synchronized static void method(){
//业务方法
}
```
3.修饰代码块(锁是括号里面的对象)
```java
synchronized (this){
//业务方法
}
```
#### 总结
- synchronized关键字驾到static静态方法和synchronized (class)代码块都是给class类上锁
- synchronized关键字加到实例方法是给对象的实例上锁


### 双重校验锁
```java
/**
 * 双重校验锁
 */
public class Singleton {
    private volatile static Singleton uniqueInstance = null;

    public Singleton()
    {

    }

    public static Singleton getUniqueInstance()
    {
        //先判断对象是否已经实例过，没有实例化过才进入加锁代码
        if(uniqueInstance == null)
        {
            //类对象加锁
            synchronized (Singleton.class)
            {
                if(uniqueInstance == null)
                {
                    uniqueInstance = new Singleton();
                }
            }
        }
        return uniqueInstance;
    }
}
```

```uniqueInstance = new Singleton()```分为3个步骤
1. 为uniqueInstance分配内存空间
2. 初始化uniqueInstance
3. 将uniqueInstance指向分配的内存地址

由于JVM具有重排特性，执行顺序可能会变成1->3->2，但是由于的volatile可以禁止JVM重拍指令，保证多线程环境下也能正常运行。

### JDK1.6之后的优化
1.6之后对锁的实现进行大量优化，利用锁升级方式减少锁的操作的开销

无锁->偏向锁->轻量级锁->自旋锁->重量级锁

> 原因是synchronized锁原来只有重量级锁，依赖操作系统的指令，需要用户态和内存态切换，性能损耗明显。引入了偏向锁和轻量级锁，就是为了在不同场景使用不同的锁，从而提高效率。

> 重量级锁用到monitor对象；而偏向锁则在Mark Word记录县城ID进行比对；轻量级锁则是拷贝MarkWord到LockRecord，用CAS+自旋方式获取。<br>
> 1. 只有一个线程进入临界区，偏向锁<br>
> 2. 多个线程交替进入临界区，轻量级锁<br>
> 3. 多线程同时进入临界区，重量级锁

- 偏向锁

  就是JVM认为只有某个线程才会执行同步的代码(没有竞争的环境)，所以再Mark Word会直接记录线程ID，只要线程来执行代码，会比对线程ID是否相等，相等则当前线程能直接获取锁，执行同步代码；如果不想等，会用CAS来尝试修改当前的线程ID，如果CAS修改成功，当前线程能直接获取锁，执行同步代码，如果CAS失败，说明有竞争环境，此时会升级为轻量级锁。

- 轻量级锁
  当前线程会在栈帧下创建LockRecord，LockRecord会把Mark word信息拷贝进去，且有个Owner指针指向加锁的对象。线程执行到同步代码时，则用CAS试图将Mark word指向到线程栈帧
  的LockRecord，假设CAS修改成功，获取到轻量级锁，如果修改失败，则自旋，自旋一定次数后，升级为重量级锁。


### synchronized与ReentrantLock区别
- 两者都是可重入锁

“可重入锁”指的是自己可以再获取自己的内部锁，比如一个线程获得了某个对象锁，此时这个对象锁还没有释放，当再次想要获取这个对象的锁还是可以获取的；如果不可重入的话，就会造成死锁。同一个线程每次获取锁，锁的计数器会自增1，所以需要锁的计数器下降为0才可以释放锁

- synchronized依赖于JVM，而ReentrantLock依赖于API

- ReentrantLock增多了一些高级功能

  1. 可等待中断：提供了可以能够终端等待锁的线程机制，正在等待锁的线程可以放弃等待
  2. 可实现公平锁：可以指定为公平锁或者非公平锁，synchronized只能是非公平锁。公平锁就是先等待的线程先获取锁。ReentrantLock默认是非公平的，通过ReentrantLock(boolean fair)构造方法可以指定是否公平


### synchronized与volatile区别

1. volatile性能比synchronized好，但是volatile只能修饰变量，synchronized可以修饰方法与代码块
2. volatile只能保证数据可见性，不能保证原子性，synchronized两者都能保证
3. volatile主要用于解决变量再多个线程之间的可见性，synchronized用于解决多个线程之间的资源同步性