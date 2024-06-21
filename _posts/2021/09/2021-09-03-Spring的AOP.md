---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Spring的AOP"
date: "2021-09-03"
categories: 
  - "Backend Spring"
---


# 介绍

> AOP是面向切面编程，是一种编程范式，通过**预编译的方式和运行期动态代理**实现程序功能的统一维护的一种技术。<br>
> 通常用于隔离不同的业务逻辑，比如事务管理、日志管理等。实现方式有两种**jdk和cglib**。

# 为什么有AOP
假设现在有几个实现方法，需要做日志处理，正常来说我们只需要手动添加一下日志就可以了，我们都知道在真正的业务代码中，代码行数，以及方法数那是一个天文数字，如果都要手动添加那工作量不现实。

AOP因此就产生了，说白了AOP就是通过某种匹配规则去匹配方法，然后再添加对应的日志处理。而AOP本身的实现方式就是通过```ASM```字节码框架动态生成技术，在程序运行的时候，根据需求（添加文件）动态创建字节码文件.

# AOP核心概念
- 切面(Aspect):似于 Java 中的类声明，常用于应用中配置事务或者日志管理。一般使用 ```@Aspect``` 注解或者``` <aop:aspect> ```来定义一个切面。
- 连接点(Join Point)：程序执行中的特定点，比如方法执行、处理一个异常等。
- 切点(Pointcut)：通过一种规则匹配的正则表达式，当有连接点可以匹配到切点时，就会触发改切点相关联的指定通知。
- 通知(Advice)：在切面中某个连接点采取的动作，通知方式也有5种：
  - around(环绕通知)：前后都加
  - before(前置通知)
  - after(后置通知)
  - exception(异常通知)
  - return(返回通知)
- 织入(Weaving)：链接切面和目标对象创建一个通知对象的过程。

> AOP实际是一种编程思想，以上的点就是该编程的具体实现规范

# AOP的实现
Spring AOP是通过动态代理技术实现，动态代理基于反射设计的。Spring AOP采用两种方式：JDK与CGLIB

{% include figure.liquid loading="eager" path="assets/img/2021/09/image-2cd77fc8e89f4208b75427963e993c24.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

> - JDK动态代理：Spring AOP的首选。每当目标对象实现了一个接口，就会使用JDK动态代理。**目标对象必须实现接口**。
> - CGLIB代理：如果目标对象没有实现接口，则可以使用CGLIB代理。

# AOP的执行过程
AOP的执行流程需要IOC作为基础

> - IOC容器启动，用来存放对象
> - 进行对象的实例化和初始化操作，将生成的完成的对象存放到容器中
> - 从创建好的容器获取需要对象
> - 调用具体的方法

> 总结：<br>
> Spring的通知，首先是通过Spring容器的启动过程获取到具体的通知，在调用对象时，通过动态代理ASM技术，把需要执行的advice先全部放到一个chain对象集合中，为了保证整个链的调用默认会先调用ExposeInvocationInterceptor(暴露调用器的拦截器)去触发整个链式的执行，在执行完每一个advice后都会再次回到super的proceed方法，继续执行下一个advice，在执行不同的advice时，有对应的切面通知方法，当所有的advice执行完毕后通过反射调用目标方法。


