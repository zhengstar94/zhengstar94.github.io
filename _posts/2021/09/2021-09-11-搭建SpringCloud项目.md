---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "搭建SpringCloud项目"
date: "2021-09-11"
categories: 
  - "Backend Spring"
---

# 搭建SpringCloud项目

## 1. New Project

{% include figure.liquid loading="eager" path="assets/img/2021/09/image-be06d1e429ec4721a986fce19f30bc36.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

## 2. 聚合总父工程名字
{% include figure.liquid loading="eager" path="assets/img/2021/09/image-c4e2b9060a5542a5a2877b67515a8468.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

## 3. Maven选择版本
{% include figure.liquid loading="eager" path="assets/img/2021/09/image-f164131ab06a454f8fe5b0a989989cf7.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

## 4. 工程名字
{% include figure.liquid loading="eager" path="assets/img/2021/09/image-90baddcad8d14e8499c7de6b64be3429.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

## 5. 字符编码
{% include figure.liquid loading="eager" path="assets/img/2021/09/image-cdd8eb1167194bae8bdadcde35a42d02.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

## 6. 注解生效激活
{% include figure.liquid loading="eager" path="assets/img/2021/09/image-0ad455c6abf74af980fe0a3f13346a6e.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

## 7. JAVA编译版本选择8
{% include figure.liquid loading="eager" path="assets/img/2021/09/image-402baf10540648ee84eade239444c2f1.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

## 8. File Type过滤
{% include figure.liquid loading="eager" path="assets/img/2021/09/image-85c2c7f3a09c4bb89d0220bb5b900584.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

## 9. 删除src文件夹

## 10. 修改pom.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.indi.springcloud</groupId>
  <artifactId>cloud2020</artifactId>
  <version>1.0-SNAPSHOT</version>
  <!--Maven的依赖-->
  <packaging>pom</packaging>

  <!-- 统一管理jar包版本 -->
  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
    <junit.version>4.12</junit.version>
    <log4j.version>1.2.17</log4j.version>
    <lombok.version>1.16.18</lombok.version>
    <mysql.version>5.1.47</mysql.version>
    <druid.version>1.1.16</druid.version>
    <mybatis.spring.boot.version>1.3.0</mybatis.spring.boot.version>
  </properties>

  <!-- 子模块继承之后，将锁定版本+子module不用写groupId和version标签  -->
  <dependencyManagement>
    <dependencies>
      <!--spring boot 2.2.2-->
      <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-dependencies</artifactId>
        <version>2.2.2.RELEASE</version>
        <type>pom</type>
        <scope>import</scope>
      </dependency>

      <!--spring cloud Hoxton.SR1-->
      <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-dependencies</artifactId>
        <version>Hoxton.SR1</version>
        <type>pom</type>
        <scope>import</scope>
      </dependency>

      <!--spring cloud alibaba 2.1.0.RELEASE-->
      <dependency>
        <groupId>com.alibaba.cloud</groupId>
        <artifactId>spring-cloud-alibaba-dependencies</artifactId>
        <version>2.1.0.RELEASE</version>
        <type>pom</type>
        <scope>import</scope>
      </dependency>

      <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>${mysql.version}</version>
      </dependency>

      <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>druid</artifactId>
        <version>${druid.version}</version>
      </dependency>

      <dependency>
        <groupId>org.mybatis.spring.boot</groupId>
        <artifactId>mybatis-spring-boot-starter</artifactId>
        <version>${mybatis.spring.boot.version}</version>
      </dependency>

      <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>${junit.version}</version>
      </dependency>

      <dependency>
        <groupId>log4j</groupId>
        <artifactId>log4j</artifactId>
        <version>${log4j.version}</version>
      </dependency>

      <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <version>${lombok.version}</version>
        <optional>true</optional>
      </dependency>
    </dependencies>
  </dependencyManagement>

  <build>
    <plugins>
      <plugin>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-maven-plugin</artifactId>
        <configuration>
          <fork>true</fork>
          <addResources>true</addResources>
        </configuration>
      </plugin>
    </plugins>
  </build>

</project>
```

