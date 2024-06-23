---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "SpringBoot"
date: "2021-09-07"
categories: 
  - "Backend Spring"
---


# Spring boot

## 问题
1. 简单介绍一下 Spring?有啥缺点?
2. 为什么要有 SpringBoot?
3. 说出使用 Spring Boot 的主要优点
4. 什么是 Spring Boot Starters?
5. Spring Boot 支持哪些内嵌 Servlet 容器？
6. 如何在 Spring Boot 应用程序中使用 Jetty 而不是 Tomcat?
7. 介绍一下@SpringBootApplication 注解
8. Spring Boot 的自动配置是如何实现的?
9. 开发 RESTful Web 服务常用的注解有哪些？
10. Spirng Boot 常用的两种配置文件
11. 什么是 YAML？YAML 配置的优势在哪？
12. Spring Boot 常用的读取配置文件的方法有哪些？
13. Spring Boot 加载配置文件的优先级了解么？
14. 常用的 Bean 映射工具有哪些?
15. Spring Boot 如何监控系统实际运行状况？
16. 1Spring Boot 如何做请求参数校验？
17. 如何使用 Spring Boot 实现全局异常处理？
18. Spring Boot 中如何实现定时任务 ?




## 介绍
Spring Boot简化Spring开发（减少配置文件，开箱即用）

## 优点
1. 容易上手
2. 开箱即用
3. 提供了一系列大型项目通用的非业务性功能
4. 配置简单

## 什么是Spring Boot starters
Spring Boot starters是一系列依赖关系的集合
在没有Spring Boot starters之前，我们开发REST服务或者Web应用程序，我们需要食用Tomcat或Jackson这样的库，依赖需要一个个手动添加，有了Spring Boot starters只需要添加这个一个依赖就可以，这个依赖中包含了我们开发REST服务所有的依赖。

## 如何在 Spring Boot 应用程序中使用 Jetty 而不是 Tomcat
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
    <exclusions>
        <exclusion>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-tomcat</artifactId>
        </exclusion>
    </exclusions>
</dependency>
<!--添加Jetty依赖-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jetty</artifactId>
</dependency>

```

## 介绍一下@SpringBootApplication注解
@SpringBootApplication看作是 @Configuration、@EnableAutoConfiguration、@ComponentScan 注解的集合

- @EnableAutoConfiguration：启用SprintBoot的自动配置机制
- @ComponentScan：扫描@Component(@Service，@Controller)注解的bean，注解默认会扫描该类所在的包下所有的类
- @ Configuration：允许在上下文中注册额外的bean或导入其他配置类


## Spring Boot的自动配置如何实现
这个是因为@SpringBootApplication的原因，由于@SpringBootApplication看作是 @Configuration、@EnableAutoConfiguration、@ComponentScan 注解的集合

```@EnableAutoConfiguration```是启动自动配置的关键

1. ```@EnableAutoConfiguration```注解通过Spring提供的@Import注解导入```@ AutoConfigurationImportSelector ```，这个类中的getCandidateConfigurations方法会将所有自动配置类的信息以List形式返回。这些配置会被Spring容器作bean来管理。
2. ```@Conditional``` 注解。@ConditionalOnClass(指定的类必须存在于类路径下)制定了容器必须还有其实现类。

{% include figure.liquid loading="eager" path="assets/img/2021/09/image-2fdea424401047dc83a0da78eebfe41a.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

### 结论
- SpringBoot 所有的自动配置都是在启动的时候扫描并加载的
- 自动配置真正实现
    - 从classpath中搜寻所有的META-INF/spring.factories配置文件
    - 并将其中对应的 org.springframework.boot.autoconfigure. 包下的配置项，通过反射实例化为对应标注了 @Configuration的JavaConfig形式的IOC容器配置类
    - 然后将这些都汇总成为一个实例并加载到IOC容器中
    - 不过有部分配置类不会一开始就自动加载，只有满足 @ConditionalOnXXX(xxxx) 中的条件才会生效 —— 导入对应的 start 启动器

## RESTful Web服务常用注解
### Spring Bean相关

- @Autowired：自动导入对象到类中，被注入进的类同样被Spring容器管理；
- @RestController : @RestController注解是@Controller和@ResponseBody的合集,表示这是个控制器 bean,并且是将函数的返回值直 接填入 HTTP 响应体中,是 REST 风格的控制器。
- @Component ：通用的注解，可标注任意类为 Spring 组件。如果一个 Bean 不知道属于哪个层，可以使用@Component 注解标注。
- @Repository : 对应持久层即 Dao 层，主要用于数据库相关操作。
- @Service : 对应服务层，主要涉及一些复杂的逻辑，需要用到 Dao 层。
- @Controller : 对应 Spring MVC 控制层，主要用于接受用户请求并调用 Service 层返回数据给前端页面。

### 常见HTTP请求类型
- @GetMapping : GET 请求、
- @PostMapping : POST 请求。
- @PutMapping : PUT 请求。
- @DeleteMapping : DELETE 请求。

### 前后端传值
- @RequestParam以及@Pathvairable ：@PathVariable用于获取路径参数，@RequestParam用于获取查询参数。
- @RequestBody ：用于读取 Request 请求（可能是 POST,PUT,DELETE,GET 请求）的 body 部分并且 Content-Type 为 application/json 格式的数据，接收到数据之后会自动将数据绑定到 Java 对象上去。系统会使用HttpMessageConverter或者自定义的HttpMessageConverter将请求的 body 中的 json 字符串转换为 java 对象。


## Spring的常用配置文件
application.properties或者 application.yml

## 读取配置文件
1. 通过@Value读取简单的配置文件信息(不推荐)
```java
@Value("${wuhan2020}")
String wuhan2020;
```

2. 通过```@ConfigurationProperties```读取并与 bean 绑定
> LibraryProperties类上加了 @Component 注解，我们可以像使用普通 bean 一样将其注入到类中使用。

```java

import lombok.Getter;
import lombok.Setter;
import lombok.ToString;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.context.annotation.Configuration;
import org.springframework.stereotype.Component;

import java.util.List;

@Component
@ConfigurationProperties(prefix = "library")
@Setter
@Getter
@ToString
class LibraryProperties {
    private String location;
    private List<Book> books;

    @Setter
    @Getter
    @ToString
    static class Book {
        String name;
        String description;
    }
}

```
**使用**

```java
package cn.javaguide.readconfigproperties;

import org.springframework.beans.factory.InitializingBean;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

/**
 * @author shuang.kou
 */
@SpringBootApplication
public class ReadConfigPropertiesApplication implements InitializingBean {

    private final LibraryProperties library;

    public ReadConfigPropertiesApplication(LibraryProperties library) {
        this.library = library;
    }

    public static void main(String[] args) {
        SpringApplication.run(ReadConfigPropertiesApplication.class, args);
    }

    @Override
    public void afterPropertiesSet() {
        System.out.println(library.getLocation());//
        System.out.println(library.getBooks());   // 
    }
}

```

3. 通过@ConfigurationProperties读取并校验
> ProfileProperties 类没有加 @Component 注解。我们在我们要使用ProfileProperties 的地方使用@EnableConfigurationProperties注册我们的配置 bean：


## @PropertySource读取指定的 properties 文件
```java

import lombok.Getter;
import lombok.Setter;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.PropertySource;
import org.springframework.stereotype.Component;

@Component
@PropertySource("classpath:website.properties")
@Getter
@Setter
class WebSite {
    @Value("${url}")
    private String url;
}

```

**使用**

```java
@Autowired
private WebSite webSite;

System.out.println(webSite.getUrl());//https://javaguide.cn/

```


## Spring Boot加载配置文件的优先级
{% include figure.liquid loading="eager" path="assets/img/2022/09/image-f2080ef5f6734df88dbde89e958dfcd2.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

## Spring Boot 如何监控系统实际运行状况

使用 Spring Boot Actuator 来对 Spring Boot 项目进行简单的监控。
```xml

<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>

```

## Spring Boot请求参数校验

### 校验注解

#### JSR提供的注解校验
- @Null 被注释的元素必须为 null
- @NotNull 被注释的元素必须不为 null
- @AssertTrue 被注释的元素必须为 true
- @AssertFalse 被注释的元素必须为 false
- @Min(value) 被注释的元素必须是一个数字，其值必须大于等于指定的最小值
- @Max(value) 被注释的元素必须是一个数字，其值必须小于等于指定的最大值
- @DecimalMin(value) 被注释的元素必须是一个数字，其值必须大于等于指定的最小值
- @DecimalMax(value) 被注释的元素必须是一个数字，其值必须小于等于指定的最大值
- @Size(max=, min=) 被注释的元素的大小必须在指定的范围内
- @Digits (integer, fraction) 被注释的元素必须是一个数字，其值必须在可接受的范围内
- @Past 被注释的元素必须是一个过去的日期
- @Future 被注释的元素必须是一个将来的日期
- @Pattern(regex=,flag=) 被注释的元素必须符合指定的正则表达式

#### Hibernate Validator提供的注解校验
- @NotBlank(message =) 验证字符串非 null，且长度必须大于 0
- @Email 被注释的元素必须是电子邮箱地址
- @Length(min=,max=) 被注释的字符串的大小必须在指定的范围内
- @NotEmpty 被注释的字符串的必须非空
- @Range(min=,max=,message=) 被注释的元素必须在合适的范围内

**使用示例**
```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class Person {

    @NotNull(message = "classId 不能为空")
    private String classId;

    @Size(max = 33)
    @NotNull(message = "name 不能为空")
    private String name;

    @Pattern(regexp = "((^Man$|^Woman$|^UGM$))", message = "sex 值不在可选范围")
    @NotNull(message = "sex 不能为空")
    private String sex;

    @Email(message = "email 格式不正确")
    @NotNull(message = "email 不能为空")
    private String email;

}

```

### 验证请求体（RequestBody）
我们在需要验证的参数上加上了@Valid 注解，如果验证失败，它将抛出MethodArgumentNotValidException。默认情况下，Spring 会将此异常转换为 HTTP Status 400（错误请求）。

```java
@RestController
@RequestMapping("/api")
public class PersonController {

    @PostMapping("/person")
    public ResponseEntity<Person> getPerson(@RequestBody @Valid Person person) {
        return ResponseEntity.ok().body(person);
    }
}

```

### 验证请求参数（Path Variavles 和 Request Parameters）
**一定一定不要忘记在类上加上 Validated 注解了，这个参数可以告诉 Spring 去校验方法参数**
```java
@RestController
@RequestMapping("/api")
@Validated
public class PersonController {

    @GetMapping("/person/{id}")
    public ResponseEntity<Integer> getPersonByID(@Valid @PathVariable("id") @Max(value = 5,message = "超过 id 的范围了") Integer id) {
        return ResponseEntity.ok().body(id);
    }

    @PutMapping("/person")
    public ResponseEntity<String> getPersonByName(@Valid @RequestParam("name") @Size(max = 6,message = "超过 name 的范围了") String name) {
        return ResponseEntity.ok().body(name);
    }
}

```

## Spring Boot实现全局异常处理？
使用```@ControllerAdvice``` 和 ```@ExceptionHandler``` 处理全局异常。

## Spring Boot实现定时任务
使用```@Scheduled``` 注解就能很方便地创建一个定时任务。

```java
@Component
public class ScheduledTasks {
    private static final Logger log = LoggerFactory.getLogger(ScheduledTasks.class);
    private static final SimpleDateFormat dateFormat = new SimpleDateFormat("HH:mm:ss");

    /**
     * fixedRate：固定速率执行。每5秒执行一次。
     */
    @Scheduled(fixedRate = 5000)
    public void reportCurrentTimeWithFixedRate() {
        log.info("Current Thread : {}", Thread.currentThread().getName());
        log.info("Fixed Rate Task : The time is now {}", dateFormat.format(new Date()));
    }
}

```
> 单纯依靠 @Scheduled 注解 还不行，我们还需要在 SpringBoot 中我们只需要在启动类上加上@EnableScheduling 注解，这样才可以启动定时任务。@EnableScheduling 注解的作用是发现注解 @Scheduled 的任务并在后台执行该任务。