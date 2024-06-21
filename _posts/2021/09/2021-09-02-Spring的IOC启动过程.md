---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Spring的IOC启动过程"
date: "2021-09-02"
categories: 
  - "Backend Spring"
---


## 介绍
Spring是一个IOC容器，容器就是放数据的，java里面的容器就是集合类，IOC容器实际就是一个map(key,value)，里面存放的是各种对象(在xml里配置的bean节点||repository、service、controller、component)，在项目启动的时候会读取配置文件里面的bean节点，根据全限定类名使用反射new对象放到map里面；扫描到上述注解的类也是通过反射new对象放到map里面。

这个时候map里面就有各种对象了，接下来我们在代码里面需要用到对象时，再通过DI注入(autowired、resource等注解，xml里bean节点内的ref属性，项目启动的时候会读取xml节点ref属性根据id注入，也会扫描这些注解，根据类型或id注入；id就是对象名)

## 解决的问题
解决了类与类之间的强引用关系，强引用关系导致了代码耦合性高，维护性差

控制反转的作用，举个例子，现在有个变量String name=“小明”，在两个类里都要用到小明这个字符串，如果不使用控制反转，在class A中要定义一个String name=“小明”；在class B中也要定义一个String name=“小明”；
这个时候我需要把小明改成李四，那我需要找到所有用到“小明”的类然后一个一个去修改。实际上我们是定义一个静态常量，然后在需要用到的地方进行引用，这样的话当我们需要把小明修改成李四只需要去定义这个静态常量的那个类里去修改一次，所有引用到的地方都被修改了。这其实就是控制反转，定义静态常量的类就是工厂类，被定义的常量就是ioc容器内的各种对象）

## 带来的问题
导致工厂类代码冗长，每增加一个接口都要在工厂类里面加一段代码

为了解决工厂模式带来的这个问题，spring通过配置化的方式来解决，也就是上面说的xml配置文件（通过遍历bean节点的方式new对象）


## 前置核心知识
1. XMl解析：IOC读取xml获取bean的相关信息，类信息、属性值信息
2. 根据第一步获取的信息，动态创建对象
- 通过**反射机制**获取到目标类的构造函数，调用构造函数<br>
- 给对象赋值


## Spring IOC容器初始化过程
1. 定位并加载配置文件；
2. 解析配置文件中的bean节点，一个bean节点对应一个BeanDefinition对象(这个对象会保存我们在Bean节点内配置的所有内容，比如id、依赖至、全限定类名等)；
3. 根据上一步的BeanDefinition集合生成(BeanDefinition对象内包含生成这个对象所需要所有的参数)所有非懒加载的单例对象，其余的会在使用的时候再实例化对应的对象；
4. 依赖注入
5. 后置处理


## Spring IOC容器初始化总结

> Spring启动 -> 加载配置文件 -> 将配置文件转化成Resource -> 从Resource中解析转换成BeanDefinition -> 主动或则被动触发Bean的初始化过程 -> 应用程序中使用Bean -> 销毁Bean -> 容器关闭。


1. Spring启动。
2. 加载配置文件，xml、JavaConfig、注解、其他形式等等，将描述我们自己定义的和Spring内置的定义的Bean加载进来。
3. 加载完配置文件后将配置文件转化成统一的Resource来处理。
4. 使用Resource解析将我们定义的一些配置都转化成Spring内部的标识形式：BeanDefinition。
5. 在低级的容器BeanFactory中，到这里就可以宣告Spring容器初始化完成了，Bean的初始化是在我们使用Bean的时候触发的；在高级的容ApplicationContext中，会自动触发那些lazy-init=false的单例Bean，让Bean以及依赖的Bean进行初始化的流程，初始化完成Bean之后高级容器也初始化完成了。
6. 在我们的应用中使用Bean。
7. Spring容器关闭，销毁各个Bean。




