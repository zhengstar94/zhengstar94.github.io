---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Spring手写实现AOP"
date: "2021-09-04"
categories: 
  - "Backend Spring"
---

# Spring手写实现AOP

最简单的办法，就是用lambda表达式

首先，先定义一个Aop类，包含对象实例化和方法调用：

```java
public class Aop {
    //根据传入的class对对象进行实例化，相当于spring的ioc功能
    public static <T> T get(Class<T> clazz) {
        try {
            return clazz.getConstructor().newInstance();
        } catch (Exception e) {
            throw new RuntimeException("获取实例化对象失败");
        }
    }
    //使用aop功能，传入一个拦截器和一个匿名方法就可以
    public static void invoke(Inteceptor interceptor,Invoke invoke){
        interceptor.invoke(invoke);
    }
}
```

然后定义两个接口，一个用做拦截器，一个用做方法的调用：

```java
public interface Inteceptor {
    public void invoke(Invoke invoke);
}

public interface Invoke {
    public void invoke();
}
```

这样就实现了最基本的Aop功能，也可以用做拦截器，一共18行代码。

要使用这个Aop的方法也很简单，下面做一个简单的演示：

```java
//这是个演示类
public class App {
    public static void main(String[] args) {
        //Hello hello=new Hello();
        Hello hello=Aop.get(Hello.class);
        Aop.invoke(new HelloInteceptor(), () ->{
            hello.test();
        });
    }
}

//这是个测试类
public class Hello{
    void test(){
        System.out.println("测试aop是否能正常使用！！！！");
    }
}
//使用方法也非常的简单，只需要调用invoke就可以执行匿名方法了。
//可以定义执行方法前后的操作，异常处理只需用try catch即可，也可自定义异常类型。
public class HelloInteceptor implements Inteceptor {
    public void invoke(Invoke invoke) {
        System.out.println("执行前");
        try {
            invoke.invoke();
        }catch (Exception e){
            System.out.println("假如出现错误");
        }
        System.out.println("执行后");
    }
}
```

