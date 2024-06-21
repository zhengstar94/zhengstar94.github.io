---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Springboot自动装配原理"
date: "2021-09-08"
categories: 
  - "Backend Spring"
---


# Springboot自动装配原理

## pom.xml
- spring-boot-dependencies :核心依赖在父工程中!
- 我们在写或者引入一些SPringboot依赖的时候,不需要指定版本,就因为有这些版本仓库


## 启动器

```xml
<dependency>
<groupid>org.springframework.boot</groupId> 
<artifactId>spring-boot-starters/artifactId> 
</dependency>
```

- 启动器:说白了就是Springboot的启动场景;
- 比如spring-boot-starter-web,他就会帮我们自动导入web环境所有的依赖!
- springboot会将所有的功能场景,都变成一个个的启动器
- 我们要使用什么功能,就只需要找到对应的启动器就可以了starter


## 主程序

```java
//@springBootApplication : 注这个类是个springboot的应用
@SpringBootApplication
publle class SpringbootelHellowor1dApplication {
	public static void main(String[] args){		 
	 //将springboot应用启动
 		SpringApplication.run(SpringbootelHelloworldApplication.class,args);
	}
}
```

## 注解（重点)

```java
@springBootConfiguration: springboot的配置
	@configuration: spring配置类
	@Component: 说明这也是一个spring的组件


@EnableAutoconfiguration :自动配置
	@AutoconfigurationPackage: 自动配置包
	@Import(AutoConfigurationPackages .Registrar.class) : 自动配置:包注册
	@Import (AutoConfiqurationImportSelector.class):自动配置导入选择

//获取所有的配置
List<string> configurations = getCandidateconfigurations(annotationMetadata, attributes);

```

获取候选的配置

```java
protected List<string> getCandidateConfigurations(AnnotationMetadata metadata,AnnotationAttributes attributes) {
List<string> configurations = SpringFactoriesLoader.loadFactoryNames(getSpringFactoriesLoaderFactoryclass(),getBeanclassLoader());
Assert.notEmpty (configurations, "No auto configuration classes found in META-INF/spring.factories. If you are using a custom packaging, make sure that file is correct.");
return configurations;

```

** META-INF/spring.factories : 自动配置的核心文件**

{% include figure.liquid loading="eager" path="assets/img/2021/09/image-770d08ac42714d2bb85a76024a974dd7.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}


```java
//所有资源加载到配置类中!
Properties properties = PropertiesLoaderutils. loadProperties(resource);
```

{% include figure.liquid loading="eager" path="assets/img/2021/09/image-1387506a5331435ba8a2e248bf353742.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

**结论**: springboot所有自动配置都是在启动的时候扫描并加载: ```spring.factories```所有的自动配置类都在这里面,但是不一定生效,要判断条件是否成立,只要导入了对应的start,就有对应的启动器了,有了启动器,我们自动装配就会生效,然后就配置成功!

1. springboot在启动的时候,从类路径下/META-INF/```spring.factories```获取指定的值;
2. 将这些自动配置的类导入容器,自动配置就会生效,帮我进行自动配置!
3. 以前我们需要自动配置的东西,现在springboot帮我们做了!
4. 整合javaEE,解决方案和自动配置的东西都在spring-boot-autoconfigure-2.2.0.RELEASE.jar这个包下
5. 它会把所有需要导入的组件,以类名的方式返回,这些组件就会被添加到容器,
6. 容器中也会存在非常多的xxXAutoConfiguration的文件(@Bean),就是这些类给容器中导入了这个场景需要 的所有组件;并自动配置,@Configuration,javaConfig
7. 有了自动配置类，免去了我们手动编写配置文件的工作!

## 启动(重点)

### Run
最初以为知识运行了一个main方法，实际上开启一个服务

```java
@SpringBootApplication
publle class SpringbootelHellowor1dApplication {
	public static void main(String[] args){		 
	 //该方法返同一个configurableApplicationcontext对象
	 //参数:应用入口的类 参数类:命令行参数
 		SpringApplication.run(SpringbootelHelloworldApplication.class,args);
	}
}
```

### SpringApplication
这个类主要做了4件事情
1. 推断应用的类型是普通的项目还是Web项目
2. 查找并加载所有可用初始化器,设置到initializers属性中
3. 找出所有的应用程序监听器,设置到listeners属性中
4. **推断并设置main方法的定义类,找到运行的主类**

{% include figure.liquid loading="eager" path="assets/img/2021/09/image-0705fba99bdf463b85ede07f64c0b78e.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}


> 重点<br>
> springboot是通过main方法下的SpringApplication.run方法启动的,启动的时候他会调用refshContext方法,先刷新容器,然后根据解析注解或者解析配置文件的形式注册bean,而它是通过启动类的SpringBootApplication注解进行开始解析的,他会根据EnableAutoConfiguration开启自动化配置,里面有个核心方法ImportSelect选择性的导入,根据loadFanctoryNames根据classpash路径以MATA-INF/spring.factorces下面以EnableAutoConfiguration开头的key去加载里面所有对应的自动化配置,他并不是把这一百二十多个自动化配置全部导入,在他每个自动化配置里面都有条件判断注解,先判断是否引入相互的jar包,再判断容器是否有bean再进行注入到bean容器








