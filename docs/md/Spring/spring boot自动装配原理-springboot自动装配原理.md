
# spring boot的自动装配原理

springbootapplication的注解下三个注解，

1. springbootconfiguration  允许在上下文中注册额外的bean
2. componentscan 扫描被component注解的bean，会扫描启动类所在包下的所有的类
3. EnableAutoconfiguration ： 这个才是启用springboot自动配置的机制



### EnableAutoconfiguration

下面包含AutoConfigurationImportSelector 实现 ImportSelector

ImportSelector实现了 selectImports

> selectImports主要用于获取符合条件的类的全限定类名，并加载到IOC中

AutoConfigurationImportSelector .getAutoConfigurationEntry

调用到SpringFactoriesLoader.loadFactoryNames 获取所有的自动配置类名

调到SpringFactoriesLoader.loadSpringFactories从```META-INF/spring.factories```加载自动配置类



`XXXAutoConfiguration`的作用就是按需加载组件



```META-INF/spring.factories```的配置不是全部一次性进行启动加载

AutoConfigurationImportSelector .getAutoConfigurationEntry方法中还有

```
configurations = filter(configurations, autoConfigurationMetadata);
```

这步用来过滤，通过注解@configuration中的类才回生效