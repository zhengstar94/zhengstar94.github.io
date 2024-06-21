---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "ElasticSearch基础"
date: "2021-11-01"
categories: 
  - "Backend ElasticSearch"
---


# 简介
ElasticSearch，简称ES，是一个开源的高扩展的分布式全文搜索引擎。它可以近乎实时的存储、检索数据；本身扩展性很好，可以扩展到上百台服务器，处理PB级别的数据。

# 优缺点
- 优点
1. Elasticsearch是分布式的。不需要其他组件，分发是实时的，被叫做”Push replication”。
2. Elasticsearch 完全支持 Apache Lucene 的接近实时的搜索。
3. 处理多租户不需要特殊配置，而Solr则需要更多的高级设置。
4. Elasticsearch 采用 Gateway 的概念，使得完备份更加简单。
5. 各节点组成对等的网络结构，某些节点出现故障时会自动分配其他节点代替其进行工作。

- 缺点
1. 提交者来自单个公司。
2. 还不够自动（不适合当前新的Index Warmup API）


# 安装
## 1. 下载软件
- [Elasticsearch 的官方地址](https://www.elastic.co/cn/)
- [下载地址](https://www.elastic.co/cn/downloads/past-releases#elasticsearch)
  {% include figure.liquid loading="eager" path="assets/img/2021/11/image-ee67614e87c4461391e6beb9a0f61139.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

- Elasticsearch 分为 Linux 和 Windows 版本，这里先下载 Windows 版本的进行学习，后面会有用到 Linux 版的。
  {% include figure.liquid loading="eager" path="assets/img/2021/11/image-2702065f656f4997a23c5914e75568d3.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

## 2. 安装软件
- Windows 版的 Elasticsearch 的安装很简单，解压即安装完毕，解压后的 Elasticsearch 的目录结构如下
  {% include figure.liquid loading="eager" path="assets/img/2021/11/image-686274f895cf448c825011cf5cb7c18d.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

- 解压后，进入 bin 文件目录，点击 elasticsearch.bat 文件启动 ES 服务

{% include figure.liquid loading="eager" path="assets/img/2021/11/image-4fe8c35995de4e3db60184bbf5b7e4cd.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

**注意：9300端口为ElasticSearch集群间组件的通信端口，9200端口为浏览器访问的http协议Restful端口**

- 启动后在浏览器中输入地址：http://localhost:9200
  {% include figure.liquid loading="eager" path="assets/img/2021/11/image-cddb12a3151a4e21b16d85ed313c4960.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}


## 3. 问题解决
1. ElasticSearch是使用java开发的，且 7.8 版本的 ES 需要 JDK 版本 1.8 以上，默认安装包带有 jdk 环境，如果系统配置 JAVA_HOME，那么使用系统默认的 JDK，如果没有配置使用自带的 JDK，一般建议使用系统配置的 JDK。
2. 双击启动窗口闪退，通过路径访问追踪错误，如果是“空间不足”，请修改config/jvm.options 配置文件

```option
# 设置 JVM 初始内存为 1G。此值可以设置与-Xmx 相同，以避免每次垃圾回收完成后 JVM 重新分配内存
# Xms represents the initial size of total heap space
# 设置 JVM 最大可用内存为 1G
# Xmx represents the maximum size of total heap space
-Xms1g
-Xmx1g
```

## 4. PostMan安装
- 在对 Elasticsearch 进行测试的过程中会使用到很多 HTTP 请求，而 HTTP 的大部分特性且仅支持 GET 和 POST 方法。所以为了能方便地进行客户端的访问，可以使用 Postman 软件。
- Postman 是一款强大的网页调试工具，提供功能强大的 Web API 和 HTTP 请求调试。软件功能强大，界面简洁明晰、操作方便快捷，设计得很人性化。Postman 中文版能够发送任何类型的 HTTP 请求 (GET, HEAD, POST, PUT…)，不仅能够表单提交，且可以附带任意类型请求体。
- [Postman 安装](https://www.getpostman.com)
- [Postman 下载](https://www.getpostman.com/apps)
  {% include figure.liquid loading="eager" path="assets/img/2021/11/image-3da18181158048feaed7d12ce3051938.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

# 数据格式
- Elasticsearch 是面向文档型数据库，一条数据在这里就是一个文档。为了方便大家理解，我们将 Elasticsearch 里存储文档数据和关系型数据库 MySQL 存储数据的概念进行一个类比。
  {% include figure.liquid loading="eager" path="assets/img/2021/11/image-57a86a68da1f42f2a78383948b1f164c.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

- ES 里的 Index 可以看做一个库，而 Types 相当于表，Documents 则相当于表的行。
- 这里 Types 的概念已经被逐渐弱化，Elasticsearch 6.X 中，一个 index 下已经只能包含一个 type，Elasticsearch 7.X 中, Type 的概念已经被删除了。

# HTTP操作

## 索引操作(Index)

### 创建索引
- 对比关系型数据库，创建索引就等同于创建数据库在 Postman 中，向 ES 服务器发 PUT 请求 ：http://127.0.0.1:9200/shopping
  {% include figure.liquid loading="eager" path="assets/img/2021/11/image-01a484bed60e4ecfb1337d62dd98a0a6.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}
- 请求后，服务器返回响应
  {% include figure.liquid loading="eager" path="assets/img/2021/11/image-e164c86ab29c455592376061dc3f157b.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

- 其中
```json
{
 "acknowledged"【响应结果】: true, # true 操作成功
 "shards_acknowledged"【分片结果】: true, # 分片操作成功
 "index"【索引名称】: "shopping"
}
```
> 注意：<br>
> 1. 创建索引库的分片数默认1片，在7.0以前，默认5片<br>
> 2. 如果重复添加索引，会返回错误信息

- 再发送一个相同请求进行测试
  {% include figure.liquid loading="eager" path="assets/img/2021/11/image-d957963d134c4feba673a02521d24fc1.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

### 查看所有的索引
- 在 Postman 中，向 ES 服务器发 ```GET``` 请求 ：http://127.0.0.1:9200/_cat/indices?v
  {% include figure.liquid loading="eager" path="assets/img/2021/11/image-20b984266e6a44118899caabec2911e2.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}
> - 这里请求路径中的_cat 表示查看的意思<br>
> - indices 表示索引<br>
> - 所以整体含义就是查看当前 ES 服务器中的所有索引，就好像 MySQL 中的 show tables 的感觉，

- 服务器响应结果如下
  {% include figure.liquid loading="eager" path="assets/img/2021/11/image-f61209901c7849c99dbdb19ddf5d43d4.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

{% include figure.liquid loading="eager" path="assets/img/2021/11/image-216e5311f6aa4e88a41554c56d6ef632.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

### 查看单个索引
- 在 Postman 中，向 ES 服务器发 ```GET``` 请求 ：http://127.0.0.1:9200/shopping
  {% include figure.liquid loading="eager" path="assets/img/2021/11/image-fae9e63c9b954464b3e426a6afe608f8.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

**查看索引向 ES 服务器发送的请求路径和创建索引是一致的。但是 HTTP 方法不一致。**

- 请求后，服务器响应结果如下
  {% include figure.liquid loading="eager" path="assets/img/2021/11/image-dd8ee136506a4c8f82b3d16d2ac9c2ac.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

### 删除索引
- 在 Postman 中，向 ES 服务器发 ```DELETE``` 请求 ：http://127.0.0.1:9200/shopping
  {% include figure.liquid loading="eager" path="assets/img/2021/11/image-88e73f3cd597487f85b4af8a769c050e.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

{% include figure.liquid loading="eager" path="assets/img/2021/11/image-5578f60da5584699b7a92210145b7293.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

- 重新访问索引时，服务器返回响应：索引不存在
  {% include figure.liquid loading="eager" path="assets/img/2021/11/image-36fe1c1eada048b882511eb65471c179.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}


## 文档操作

### 创建文档
- 索引已经创建好了，接下来我们来创建文档，并添加数据。这里的文档可以类比为关系型数据库中的表数据，添加的数据格式为 JSON 格式

- 在 Postman 中，向 ES 服务器发 ```POST``` 请求 ：http://127.0.0.1:9200/shopping/phone

{% include figure.liquid loading="eager" path="assets/img/2021/11/image-564b9ee73711492a9a1aa1583a4f7678.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

- 此处发送请求的方式必须为 POST，不能是 PUT，否则会发生错误，错误的返回结果为：
  {% include figure.liquid loading="eager" path="assets/img/2021/11/image-351ebde1e01c402788db6f743340e80d.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

- 正确响应结果如下
  {% include figure.liquid loading="eager" path="assets/img/2021/11/image-28c4c4375f4941bfb8174a142675a2cd.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

- 上面的数据创建后，由于没有指定数据唯一性标识（ID），默认情况下，ES 服务器会随机生成一个。

- 如果想要自定义唯一性标识，需要在创建时指定：http://127.0.0.1:9200/shopping/phone/1
  {% include figure.liquid loading="eager" path="assets/img/2021/11/image-39cf173a93a64755a46975e81f28be9f.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

{% include figure.liquid loading="eager" path="assets/img/2021/11/image-7b19b9388aa246e0b659250f3c0cde8d.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

- 此处需要注意：如果增加数据时明确数据主键，那么请求方式可以为PUT

### 查看文档
- 查看文档时，需要指明文档的唯一性标识，类似于 MySQL 中数据的主键查询
- 在 Postman 中，向 ES 服务器发 ```GET``` 请求 ：http://127.0.0.1:9200/shopping/phone/1

{% include figure.liquid loading="eager" path="assets/img/2021/11/image-63fa7f6e3e754242b4b01b7143ec1e3e.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

- 查询成功后，服务器响应结果：
  {% include figure.liquid loading="eager" path="assets/img/2021/11/image-4a3ba0cad8cf40d283cea5d9e0f7af79.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

### 修改文档
- 和新增文档一样，输入相同的 URL 地址请求，如果请求体变化，会将原有的数据内容覆盖

- 在 Postman 中，向 ES 服务器发 ```POST``` 请求 ：http://127.0.0.1:9200/shopping/phone/1

{% include figure.liquid loading="eager" path="assets/img/2021/11/image-e28a4aa7528b43b98a399f6d4d22d41e.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

```json
{
 "title":"华为手机",
 "category":"华为",
 "images":"http://www.gulixueyuan.com/hw.jpg",
 "price":4999.00
}
```

- 修改成功后，服务器响应结果：
  {% include figure.liquid loading="eager" path="assets/img/2021/11/image-8e2a888b3ce54ff688aa424204a3107f.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

### 修改字段
- 修改数据时，也可以只修改某一给条数据的局部信息

- 在 Postman 中，向 ES 服务器发 POST 请求 ：http://127.0.0.1:9200/shopping/_update/1
  {% include figure.liquid loading="eager" path="assets/img/2021/11/image-eb954da73f45448db6d4f656fef4569a.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

- 修改成功后，服务器响应结果
  {% include figure.liquid loading="eager" path="assets/img/2021/11/image-0e491f6944984b7a95ff571f0a7cbe4a.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

- 根据唯一性标识，查询文档数据，文档数据已经更新
  {% include figure.liquid loading="eager" path="assets/img/2021/11/image-da2c826e1f3f452e90ae9658d9102e38.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}


### 按照编号删除文档
- 删除一个文档不会立即从磁盘上移除，它只是被标记成已删除（逻辑删除）。

- 在 Postman 中，向 ES 服务器发 ```DELETE``` 请求 ：http://127.0.0.1:9200/shopping/phone/1
  {% include figure.liquid loading="eager" path="assets/img/2021/11/image-4989378ad8e744fd8f5190f4b800e445.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

- 删除成功，服务器响应结果：
  {% include figure.liquid loading="eager" path="assets/img/2021/11/image-f5e38712725142e891b00bb5ef775e19.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

- 删除后再查看当前文档信息
  {% include figure.liquid loading="eager" path="assets/img/2021/11/image-04efa21c5ced414eb1b35ff94881ce16.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

- 如果删除一个不存在的文档
  {% include figure.liquid loading="eager" path="assets/img/2021/11/image-789bce4b84ec4c108fb30595e9ff17c3.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

### 条件删除文档
- 一般删除数据都是根据文档的唯一性标识进行删除，实际操作时，也可以根据条件对多条数据进行删除

- 首先分别增加多条数据:
  {% include figure.liquid loading="eager" path="assets/img/2021/11/image-8827bcdc288f4ff1ac64c3e7c080c381.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

- 开始条件删除，向 ES 服务器发 ```POST```请求 ：http://127.0.0.1:9200/shopping/_delete_by_query
  {% include figure.liquid loading="eager" path="assets/img/2021/11/image-d1030f5732844333b9403d4d96c59ef6.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

- 删除成功后，服务器响应结果
  {% include figure.liquid loading="eager" path="assets/img/2021/11/image-547f77d276f14aa4be640e868be88cd7.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

## 映射操作
- 有了索引库，等于有了数据库中的 database。
- 接下来就需要建索引库(index)中的映射了，类似于数据库(database)中的表结构(table)。创建数据库表需要设置字段名称，类型，长度，约束等；索引库也一样，需要知道这个类型下有哪些字段，每个字段有哪些约束信息，这就叫做映射(mapping)

### 创建映射
在 Postman 中，向 ES 服务器发 ```PUT``` 请求 ：http://127.0.0.1:9200/student/_mapping
{% include figure.liquid loading="eager" path="assets/img/2021/11/image-1ed994aadffe4be18a6c2597e70679c9.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

```json
{
	 "properties": {
		 "name":{
			 "type": "text",
			 "index": true
		 },
		 "sex":{
			 "type": "text",
			 "index": false
		 },
		 "age":{
			 "type": "long",
			 "index": false
		 }
	 } 
}
```

- 服务器响应结果如下
  {% include figure.liquid loading="eager" path="assets/img/2021/11/image-630af19b9cca468dbb6bdc5754808881.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

- 映射数据说明

> 字段名：任意填写，下面指定许多属性，例如：title、subtitle、images、price
> - type：类型，Elasticsearch 中支持的数据类型非常丰富，说几个关键的：
    - String 类型，又分两种：
    - text：可分词
    - keyword：不可分词，数据会作为完整字段进行匹配
    - Numerical：数值类型，分两类
    - 基本数据类型：long、integer、short、byte、double、float、half_float
    - 浮点数的高精度类型：scaled_float
    - Date：日期类型
    - Array：数组类型
    - Object：对象
> - index：是否索引，默认为 true，也就是说你不进行任何配置，所有字段都会被索引。
    - true：字段会被索引，则可以用来进行搜索
    - false：字段不会被索引，不能用来搜索
> - store：是否将数据进行独立存储，默认为 false
    - 原始的文本会存储在_source 里面，默认情况下其他提取出来的字段都不是独立存储的，是从_source 里面提取出来的。当然你也可以独立的存储某个字段，只要设置"store": true 即可，获取独立存储的字段要比从_source 中解析快得多，但是也会占用更多的空间，所以要根据实际业务需求来设置。
> - analyzer：分词器，这里的 ik_max_word 即使用 ik 分词器,后面会有专门的章节学习

### 查看映射
- 在 Postman 中，向 ES 服务器发 ```GET``` 请求 ：http://127.0.0.1:9200/shopping/_mapping
  {% include figure.liquid loading="eager" path="assets/img/2021/11/image-1e51ee10d2654a6cbebc75fd849e7b99.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

- 服务器响应结果如下
  {% include figure.liquid loading="eager" path="assets/img/2021/11/image-6ba19f24b10648b385318b87b0f8791c.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}


### 索引映射关联
- 在 Postman 中，向 ES 服务器发 PUT 请求 ：http://127.0.0.1:9200/student1
  {% include figure.liquid loading="eager" path="assets/img/2021/11/image-f443e3d54ea34007aa59f491795d9b2f.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}
- 服务器响应结果如下
  {% include figure.liquid loading="eager" path="assets/img/2021/11/image-16c2bc1bca0b42eb82056519314bd86b.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}


## 高级查询
- Elasticsearch 提供了基于 JSON 提供完整的查询 DSL 来定义查询

- 定义数据 :

```json
# POST /student/_doc/1001
{
	"name":"zhangsan",
	"nickname":"zhangsan",
	"sex":"男",
	"age":30
}
# POST /student/_doc/1002
{
	"name":"lisi",
	"nickname":"lisi",
	"sex":"男",
	"age":20 
}
# POST /student/_doc/1003
{
	"name":"wangwu",
	"nickname":"wangwu",
	"sex":"女",
	"age":40 
}
# POST /student/_doc/1004
{
	"name":"zhangsan1",
	"nickname":"zhangsan1",
	"sex":"女",
	"age":50 
}
# POST /student/_doc/1005
{
	"name":"zhangsan2",
	"nickname":"zhangsan2",
	"sex":"女",
	"age":30 
}
```

### 查询所有文档
-  在 Postman 中，向 ES 服务器发 ```GET``` 请求 ：http://127.0.0.1:9200/student/_search
-
```json
 "query": {
	 "match_all": {}
	 } 
 }
# "query"：这里的 query 代表一个查询对象，里面可以有不同的查询属性
# "match_all"：查询类型，例如：match_all(代表查询所有)， match，term ， range 等等
# {查询条件}：查询条件会根据类型的不同，写法也有差异
```
{% include figure.liquid loading="eager" path="assets/img/2021/11/image-6fa610ffb28e42a38157e64c94174beb.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

- 服务器响应结果如下：
  {% include figure.liquid loading="eager" path="assets/img/2021/11/image-de0d8340e6934a3397820fbb19162515.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

### 匹配查询
- match 匹配类型查询，会把查询条件进行分词，然后进行查询，多个词条之间是 or 的关系

- 在 Postman 中，向 ES 服务器发 GET 请求 ：http://127.0.0.1:9200/student/_search
```json
{
	"query": {
		"match": {
			"name":"zhangsan"
		}
	} 
}
```
{% include figure.liquid loading="eager" path="assets/img/2021/11/image-5ab61b953fe4402b9f4798b0feeec051.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

- 服务器响应结果为：
  {% include figure.liquid loading="eager" path="assets/img/2021/11/image-d84d586ffe614c3389c1cd789cd59f2c.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

### 字段匹配查询
- multi_match 与 match 类似，不同的是它可以在多个字段中查询。

- 在 Postman 中，向 ES 服务器发 GET 请求 ：http://127.0.0.1:9200/student/_search
```json
{
	"query": {
		"multi_match": {
			"query": "zhangsan",
			"fields": ["name","nickname"]
		}
	} 
}
```
{% include figure.liquid loading="eager" path="assets/img/2021/11/image-d8878f6e83b344db902f6ac7271ebe78.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

- 服务器响应结果
  {% include figure.liquid loading="eager" path="assets/img/2021/11/image-fa5e6f7f50364309abb8177cd39ed432.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

### 关键字精确查询
- term 查询，精确的关键词匹配查询，不对查询条件进行分词。

- 在 Postman 中，向 ES 服务器发 GET 请求 ：http://127.0.0.1:9200/student/_search
```json
{
	"query": {
		"term": {
			"name": {
				"value": "zhangsan"
			}
		}
	}
}
```
{% include figure.liquid loading="eager" path="assets/img/2021/11/image-a8f99894f290493ca00144d1f9fbceee.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

- 服务器响应结果
  {% include figure.liquid loading="eager" path="assets/img/2021/11/image-45991c2df50d45e1bc7bf3ece03b258a.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

### 多关键字精确查询
- terms 查询和 term 查询一样，但它允许你指定多值进行匹配。

- 如果这个字段包含了指定值中的任何一个值，那么这个文档满足条件，类似于 mysql 的 in

- 在 Postman 中，向 ES 服务器发 GET 请求 ：http://127.0.0.1:9200/student/_search
```json
{
	"query": {
		"terms": {
			"name": ["zhangsan","lisi"]
		}
	} 
}
```
{% include figure.liquid loading="eager" path="assets/img/2021/11/image-64c9ff574b6247b29763763f24695015.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

- 结果为：
  {% include figure.liquid loading="eager" path="assets/img/2021/11/image-67fa105dad8b4ae7bac016feff545a19.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

### 指定查询字段
-  默认情况下，Elasticsearch 在搜索的结果中，会把文档中保存在_source 的所有字段都返回。

- 如果我们只想获取其中的部分字段，我们可以添加_source 的过滤

- 在 Postman 中，向 ES 服务器发 GET 请求 ：http://127.0.0.1:9200/student/_search
```json
{
	"_source": ["name","nickname"], 
	"query": {
		"terms": {
			"nickname": ["zhangsan"]
		}
	} 
}
```
{% include figure.liquid loading="eager" path="assets/img/2021/11/image-c5e2048f01134afb82784179ec7d5b97.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

- 返回的结果
  {% include figure.liquid loading="eager" path="assets/img/2021/11/image-993930b554a3465f85ce5cdae95f5c55.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

### 过滤字段
- 如何指定不显示那些字段

- 在 Postman 中，向 ES 服务器发 GET 请求 ：http://127.0.0.1:9200/student/_search
```json
{
	"_source": {
		"excludes": ["name","nickname"]
	}, 
	"query": {
		"terms": {
			"nickname": ["zhangsan"]
		}
	}
}
```

> - includes：来指定想要显示的字段,(可以理解为默认为 includes，所以只配置显示那些字段时可省略)
> - excludes：来指定不想要显示的字段
    {% include figure.liquid loading="eager" path="assets/img/2021/11/image-a38b88244db148628fe580e09c5a031a.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}


- 结果为

{% include figure.liquid loading="eager" path="assets/img/2021/11/image-5231c51ea38c4dcd9a11d0a6514752e4.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

### 组合查询
- 通过 bool 关键字，将查询条件进行组合，其中：
> - must: 必须
> - must_not: 必须不
> - should: 应该

- 在 Postman 中，向 ES 服务器发 GET 请求 ：http://127.0.0.1:9200/student/_search
```json
{
    "query":{
        "bool":{
            "must":[
                {
                    "match":{
                        "name":"zhangsan"
                    }
                }
            ],
            "must_not":[
                {
                    "match":{
                        "age":"40"
                    }
                }
            ],
            "should":[
                {
                    "match":{
                        "sex":"男"
                    }
                }
            ]
        }
    }
}
```

{% include figure.liquid loading="eager" path="assets/img/2021/11/image-6b8617e86fd646be908827e9c53fa745.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

- 结果为
  {% include figure.liquid loading="eager" path="assets/img/2021/11/image-3657a6e1ec814531bf5dbfe03abb3a26.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

### 范围查询
- range 查询找出那些落在指定区间内的数字或者时间。range 查询允许以下字符
  {% include figure.liquid loading="eager" path="assets/img/2021/11/image-f45d9cd2f8d34f37b420579b30732336.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

- 在 Postman 中，向 ES 服务器发 GET 请求 ：http://127.0.0.1:9200/student/_search
```json
{
    "query":{
        "range":{
            "age":{
                "gte":30,
                "lte":35
            }
        }
    }
}
```
{% include figure.liquid loading="eager" path="assets/img/2021/11/image-3a7b980c611144e18c8785db5edf872e.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

- 结果为：
  {% include figure.liquid loading="eager" path="assets/img/2021/11/image-bed45690af9b4e188fe19ef26a857b55.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

### 模糊查询
- 返回包含与搜索字词相似的字词的文档。

- 编辑距离是将一个术语转换为另一个术语所需的一个字符更改的次数。这些更改可以包括：

> - 更改字符（box → fox）
> - 删除字符（black → lack）
> - 插入字符（sic → sick）
> - 转置两个相邻字符（act → cat）

- 为了找到相似的术语，fuzzy 查询会在指定的编辑距离内创建一组搜索词的所有可能的变体或扩展。然后查询返回每个扩展的完全匹配。

- 通过 fuzziness 修改编辑距离。一般使用默认值 AUTO，根据术语的长度生成编辑距离。

- 在 Postman 中，向 ES 服务器发 GET 请求 ：
```json
{
    "query":{
        "fuzzy":{
            "name":{
                "value":"zhangsan"
            }
        }
    }
}
```
{% include figure.liquid loading="eager" path="assets/img/2021/11/image-871d092833e5496d9b02561eeeed011a.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

- 结果为
  {% include figure.liquid loading="eager" path="assets/img/2021/11/image-f9a65dd8c6124b7185fe572db3b2000c.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

### 字段排序
- sort 可以让我们按照不同的字段进行排序，并且通过 order 指定排序的方式。desc 降序，asc 升序。

- 在 Postman 中，向 ES 服务器发 GET 请求 ：http://127.0.0.1:9200/student/_search
```json
{
    "query":{
        "match_all":{

        }
    },
    "sort":[
        {
            "age":{
                "order":"desc"
            }
        },
        {
            "_score":{
                "order":"desc"
            }
        }
    ]
}
```
{% include figure.liquid loading="eager" path="assets/img/2021/11/image-18b644f78976485295c6a2a6b8721441.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

- 结果为：
  {% include figure.liquid loading="eager" path="assets/img/2021/11/image-f96e95fa7edd4bee8b34131f4d6537c8.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

### 高亮查询
- Elasticsearch 可以对查询内容中的关键字部分，进行标签和样式(高亮)的设置。

- 在使用 match 查询的同时，加上一个 highlight 属性：

> - pre_tags：前置标签
> - post_tags：后置标签
> - fields：需要高亮的字段
> - title：这里声明 title 字段需要高亮，后面可以为这个字段设置特有配置，也可以空

- 在 Postman 中，向 ES 服务器发 GET 请求 ：http://127.0.0.1:9200/student/_search
```json
{
    "query":{
        "match":{
            "name":"zhangsan"
        }
    },
    "highlight":{
        "pre_tags":"<font color='red'>",
        "post_tags":"</font>",
        "fields":{
            "name":{

            }
        }
    }
}
```

- 结果为：
  {% include figure.liquid loading="eager" path="assets/img/2021/11/image-ec589fad3dbf4647a9750c1ef723730b.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}


### 分页查询
- from：当前页的起始索引，默认从 0 开始。

- size：每页显示多少条

- from = (pageNum - 1) * size

- 在 Postman 中，向 ES 服务器发 GET 请求 ：http://127.0.0.1:9200/student/_search

```json
{
    "query":{
        "match_all":{

        }
    },
    "sort":[
        {
            "age":{
                "order":"desc"
            }
        }
    ],
    "from":0,
    "size":2
}
```

- 结果为：
  {% include figure.liquid loading="eager" path="assets/img/2021/11/image-26e3aeb97a9c480ba0897d0685ecff3c.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

### 聚合查询
- 聚合允许使用者对 es 文档进行统计分析，类似与关系型数据库中的 group by，当然还有很
  多其他的聚合，例如取最大值、平均值等等。

- 在 Postman 中，向 ES 服务器发 GET 请求 ：http://127.0.0.1:9200/student/_search

```json
{
    "aggs":{
        "max_age":{
            "max":{
                "field":"age"
            }
        }
    },
    "size":0
}
```
- 结果为：
  {% include figure.liquid loading="eager" path="assets/img/2021/11/image-4b053cd71ddf41d69d1a2cba3031e537.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}



## JAVA API操作
- Elasticsearch 软件是由 Java 语言开发的，所以也可以通过 Java API 的方式对 Elasticsearch服务进行访问

### 搭建环境
1. 在 IDEA 中创建一个 Maven 依赖
2. 修改 Pom.xml

```xml
<dependencies>
    <dependency>
        <groupId>org.elasticsearch</groupId>
        <artifactId>elasticsearch</artifactId>
        <version>7.8.0</version>
    </dependency>
    <!-- elasticsearch的客户端 -->
    <dependency>
        <groupId>org.elasticsearch.client</groupId>
        <artifactId>elasticsearch-rest-high-level-client</artifactId>
        <version>7.8.0</version>
    </dependency>
    <!-- elasticsearch依赖2.x的log4j -->
    <dependency>
        <groupId>org.apache.logging.log4j</groupId>
        <artifactId>log4j-api</artifactId>
        <version>2.8.2</version>
    </dependency>
    <dependency>
        <groupId>org.apache.logging.log4j</groupId>
        <artifactId>log4j-core</artifactId>
        <version>2.8.2</version>
    </dependency>
    <dependency>
        <groupId>com.fasterxml.jackson.core</groupId>
        <artifactId>jackson-databind</artifactId>
        <version>2.9.9</version>
    </dependency>
    <!-- junit单元测试 -->
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.12</version>
    </dependency>
</dependencies>
```

### 创建客户端对象
- 新建一个类，并编写代码

```java
public class ESTest_Client {
    public static void main(String[] args) throws Exception {
        // 创建ES客户端
        RestHighLevelClient esClient = new RestHighLevelClient(
                RestClient.builder(new HttpHost("localhost", 9200, "http"))
        );
        // 关闭ES客户端
        esClient.close();
    }
}
```
**注意：9200 端口为 Elasticsearch 的 Web 通信端口，localhost 为启动 ES 服务的主机名**

- 启动测试
  控制台没有报错就表示连接成功

### 索引操作
- ES 服务器正常启动后，可以通过 Java API 客户端对象对 ES 索引进行操作

1.创建索引

  - 创建新的类

```java
public class ESTest_Index_Create {
    public static void main(String[] args) throws Exception {
        RestHighLevelClient esClient = new RestHighLevelClient(
                RestClient.builder(new HttpHost("localhost", 9200, "http"))
        );
        // 创建索引
        CreateIndexRequest request = new CreateIndexRequest("user");
        CreateIndexResponse createIndexResponse =
                esClient.indices().create(request, RequestOptions.DEFAULT);
        // 响应状态
        boolean acknowledged = createIndexResponse.isAcknowledged();
        System.out.println("索引操作 ：" + acknowledged);
        esClient.close();
    }
}
```


- 启动测试
  {% include figure.liquid loading="eager" path="assets/img/2021/11/image-a0851ac47a7849f09ad350e544845831.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

2.查看索引
```java
// 查询索引
    GetIndexRequest request = new GetIndexRequest("user");

    GetIndexResponse getIndexResponse =
            esClient.indices().get(request, RequestOptions.DEFAULT);
    // 响应状态
    System.out.println(getIndexResponse.getAliases());
    System.out.println(getIndexResponse.getMappings());
    System.out.println(getIndexResponse.getSettings());
```

{% include figure.liquid loading="eager" path="assets/img/2021/11/image-ef30d45883c549a99ccb770a69c351da.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

3.删除索引

```java
 // 查询索引
    DeleteIndexRequest request = new DeleteIndexRequest("user");
    AcknowledgedResponse response = esClient.indices().delete(request, RequestOptions.DEFAULT);
    // 响应状态
    System.out.println(response.isAcknowledged());
```
{% include figure.liquid loading="eager" path="assets/img/2021/11/image-585c2d4f8b314d13bff0bf9901ab5b35.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

### 文档操作
- 创建数据模型

```java
public class User {
    private String name;
    private String sex;
    private Integer age;
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public String getSex() {
        return sex;
    }
    public void setSex(String sex) {
        this.sex = sex;
    }
    public Integer getAge() {
        return age;
    }
    public void setAge(Integer age) {
        this.age = age;
    }
}
```

1.新增文档

```java
 // 插入数据
    IndexRequest request = new IndexRequest();
    request.index("user").id("1001");

    User user = new User();
    user.setName("zhangsan");
    user.setAge(30);
    user.setSex("男");

    // 向ES插入数据，必须将数据转换位JSON格式
    ObjectMapper mapper = new ObjectMapper();
    String userJson = mapper.writeValueAsString(user);
    request.source(userJson, XContentType.JSON);

    IndexResponse response = esClient.index(request, RequestOptions.DEFAULT);

    System.out.println(response.getResult());
```
{% include figure.liquid loading="eager" path="assets/img/2021/11/image-91d99253d09749bab1119351017aa55f.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}


2.修改文档

```java
// 修改数据
    UpdateRequest request = new UpdateRequest();
    request.index("user").id("1001");
    request.doc(XContentType.JSON, "sex", "女");

    UpdateResponse response = esClient.update(request, RequestOptions.DEFAULT);

    System.out.println(response.getResult());

```

{% include figure.liquid loading="eager" path="assets/img/2021/11/image-2a6940c1e8cd4521ba46a8ad3a6c9c49.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

3.查询文档

```java
// 查询数据
    GetRequest request = new GetRequest();
    request.index("user").id("1001");
    GetResponse response = esClient.get(request, RequestOptions.DEFAULT);

    System.out.println(response.getSourceAsString());
```
{% include figure.liquid loading="eager" path="assets/img/2021/11/image-95912088c18f4608aa0e9c2bccbf8026.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

4.删除文档

```java
	DeleteRequest request = new DeleteRequest();
    request.index("user").id("1001");

    DeleteResponse response = esClient.delete(request, RequestOptions.DEFAULT);
    System.out.println(response.toString());
```
{% include figure.liquid loading="eager" path="assets/img/2021/11/image-e67beb36bd4245429a0daffcc5dd2a80.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

5.批量操作

- 批量新增

```java
// 批量插入数据
    BulkRequest request = new BulkRequest();

    request.add(new IndexRequest().index("user").id("1001").source(XContentType.JSON, "name", "zhangsan", "age",30,"sex","男"));
    request.add(new IndexRequest().index("user").id("1002").source(XContentType.JSON, "name", "lisi", "age",30,"sex","女"));
    request.add(new IndexRequest().index("user").id("1003").source(XContentType.JSON, "name", "wangwu", "age",40,"sex","男"));

    BulkResponse response = esClient.bulk(request, RequestOptions.DEFAULT);
    System.out.println(response.getTook());
    System.out.println(response.getItems());
```
{% include figure.liquid loading="eager" path="assets/img/2021/11/image-3b5b20c7cb6f4751b69cd5506d7d6198.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}


- 批量删除

```java
// 批量删除数据
    BulkRequest request = new BulkRequest();

    request.add(new DeleteRequest().index("user").id("1001"));
    request.add(new DeleteRequest().index("user").id("1002"));
    request.add(new DeleteRequest().index("user").id("1003"));

    BulkResponse response = esClient.bulk(request, RequestOptions.DEFAULT);
    System.out.println(response.getTook());
    System.out.println(response.getItems());
```
{% include figure.liquid loading="eager" path="assets/img/2021/11/image-5e1d75dd715849bcb739a5c180744e58.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}


### 高级查询
1.查询所有索引数据

```java
// 1. 查询索引中全部的数据
    SearchRequest request = new SearchRequest();
    request.indices("user");
    request.source(new SearchSourceBuilder().query(QueryBuilders.matchAllQuery()));
    SearchResponse response = esClient.search(request, RequestOptions.DEFAULT);
    SearchHits hits = response.getHits();
    System.out.println(hits.getTotalHits());
    System.out.println(response.getTook());
    for ( SearchHit hit : hits ) {
        System.out.println(hit.getSourceAsString());
    }
```
{% include figure.liquid loading="eager" path="assets/img/2021/11/image-467427eb3b824687ad173e2981b58a88.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

2.term查询，查询条件为关键字

```java
	SearchRequest request = new SearchRequest();
    request.indices("user");
    request.source(new SearchSourceBuilder().query(QueryBuilders.termQuery("age", 30)));
    SearchResponse response = esClient.search(request, RequestOptions.DEFAULT);
    SearchHits hits = response.getHits();
    System.out.println(hits.getTotalHits());
    System.out.println(response.getTook());
    for ( SearchHit hit : hits ) {
        System.out.println(hit.getSourceAsString());
    }
```
{% include figure.liquid loading="eager" path="assets/img/2021/11/image-462a2bafb3664c2c907600babf0268aa.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

3.分页查询

```java
	SearchRequest request = new SearchRequest();
    request.indices("user");
    SearchSourceBuilder builder = new SearchSourceBuilder().query(QueryBuilders.matchAllQuery());
    // (当前页码-1)*每页显示数据条数
    builder.from(2);
    builder.size(2);
    request.source(builder);
    SearchResponse response = esClient.search(request, RequestOptions.DEFAULT);
    SearchHits hits = response.getHits();
    System.out.println(hits.getTotalHits());
    System.out.println(response.getTook());
    for ( SearchHit hit : hits ) {
        System.out.println(hit.getSourceAsString());
    }
```
{% include figure.liquid loading="eager" path="assets/img/2021/11/image-ce3c0258e2224c859e3fc838f24f819a.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

4.查询排序

```java
	SearchRequest request = new SearchRequest();
    request.indices("user");
    SearchSourceBuilder builder = new SearchSourceBuilder().query(QueryBuilders.matchAllQuery());
    builder.sort("age", SortOrder.DESC);
    request.source(builder);
    SearchResponse response = esClient.search(request, RequestOptions.DEFAULT);
    SearchHits hits = response.getHits();
    System.out.println(hits.getTotalHits());
    System.out.println(response.getTook());
    for ( SearchHit hit : hits ) {
        System.out.println(hit.getSourceAsString());
    }
```

{% include figure.liquid loading="eager" path="assets/img/2021/11/image-2a6892ccb7a449539b47cfdc9390c74e.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

5.过滤字段

```java
	SearchRequest request = new SearchRequest();
    request.indices("user");
    SearchSourceBuilder builder = new SearchSourceBuilder().query(QueryBuilders.matchAllQuery());
    String[] excludes = {"age"};
    String[] includes = {};
    builder.fetchSource(includes, excludes);
    request.source(builder);
    SearchResponse response = esClient.search(request, RequestOptions.DEFAULT);
    SearchHits hits = response.getHits();
    System.out.println(hits.getTotalHits());
    System.out.println(response.getTook());
    for ( SearchHit hit : hits ) {
        System.out.println(hit.getSourceAsString());
    }
```
{% include figure.liquid loading="eager" path="assets/img/2021/11/image-0129d59a117040a896712526ffc3938d.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

6.组合查询

```java
	SearchRequest request = new SearchRequest();
    request.indices("user");
    SearchSourceBuilder builder = new SearchSourceBuilder();
    BoolQueryBuilder boolQueryBuilder = QueryBuilders.boolQuery();
    //boolQueryBuilder.must(QueryBuilders.matchQuery("age", 30));
    //boolQueryBuilder.must(QueryBuilders.matchQuery("sex", "男"));
    //boolQueryBuilder.mustNot(QueryBuilders.matchQuery("sex", "男"));
    boolQueryBuilder.should(QueryBuilders.matchQuery("age", 30));
    boolQueryBuilder.should(QueryBuilders.matchQuery("age", 40));
    builder.query(boolQueryBuilder);
    request.source(builder);
    SearchResponse response = esClient.search(request, RequestOptions.DEFAULT);
    SearchHits hits = response.getHits();
    System.out.println(hits.getTotalHits());
    System.out.println(response.getTook());
    for ( SearchHit hit : hits ) {
        System.out.println(hit.getSourceAsString());
    }
```
{% include figure.liquid loading="eager" path="assets/img/2021/11/image-fa9f177b4cb541498f2f4d2e6eb1e969.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

7.范围查询
```java
	SearchRequest request = new SearchRequest();
    request.indices("user");

    SearchSourceBuilder builder = new SearchSourceBuilder();
    RangeQueryBuilder rangeQuery = QueryBuilders.rangeQuery("age");

    rangeQuery.gte(30);
    rangeQuery.lt(50);

    builder.query(rangeQuery);

    request.source(builder);
    SearchResponse response = esClient.search(request, RequestOptions.DEFAULT);

    SearchHits hits = response.getHits();

    System.out.println(hits.getTotalHits());
    System.out.println(response.getTook());

    for ( SearchHit hit : hits ) {
        System.out.println(hit.getSourceAsString());
    }
```
{% include figure.liquid loading="eager" path="assets/img/2021/11/image-d7557970789041a9b95f729ddced55f3.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

8.模糊查询

```java
	SearchRequest request = new SearchRequest();
    request.indices("user");

    SearchSourceBuilder builder = new SearchSourceBuilder();
    builder.query(QueryBuilders.fuzzyQuery("name", "wangwu").fuzziness(Fuzziness.TWO));

    request.source(builder);
    SearchResponse response = esClient.search(request, RequestOptions.DEFAULT);

    SearchHits hits = response.getHits();

    System.out.println(hits.getTotalHits());
    System.out.println(response.getTook());

    for ( SearchHit hit : hits ) {
        System.out.println(hit.getSourceAsString());
    }

```

{% include figure.liquid loading="eager" path="assets/img/2021/11/image-dc3b5962a82e469d9c27f7279fe8670e.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}


9.高亮查询

```java
	SearchRequest request = new SearchRequest();
    request.indices("user");

    SearchSourceBuilder builder = new SearchSourceBuilder();
    TermsQueryBuilder termsQueryBuilder = QueryBuilders.termsQuery("name", "zhangsan");

    HighlightBuilder highlightBuilder = new HighlightBuilder();
    highlightBuilder.preTags("<font color='red'>");
    highlightBuilder.postTags("</font>");
    highlightBuilder.field("name");

    builder.highlighter(highlightBuilder);
    builder.query(termsQueryBuilder);

    request.source(builder);
    SearchResponse response = esClient.search(request, RequestOptions.DEFAULT);

    SearchHits hits = response.getHits();

    System.out.println(hits.getTotalHits());
    System.out.println(response.getTook());

    for ( SearchHit hit : hits ) {
        System.out.println(hit.getSourceAsString());
        System.out.println(hit.getHighlightFields());
    }
```
{% include figure.liquid loading="eager" path="assets/img/2021/11/image-174fd62541384e2fa497ecfaeea7d520.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

10.聚合查询
```java
	SearchRequest request = new SearchRequest();
    request.indices("user");

    SearchSourceBuilder builder = new SearchSourceBuilder();

    AggregationBuilder aggregationBuilder = AggregationBuilders.max("maxAge").field("age");
    builder.aggregation(aggregationBuilder);

    request.source(builder);
    SearchResponse response = esClient.search(request, RequestOptions.DEFAULT);

    SearchHits hits = response.getHits();

    System.out.println(hits.getTotalHits());
    System.out.println(response.getTook());

    for ( SearchHit hit : hits ) {
        System.out.println(hit.getSourceAsString());
    }

```

{% include figure.liquid loading="eager" path="assets/img/2021/11/image-4acbe36e38e14f1d825098208fdcc215.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

11.分组查询

```java
	SearchRequest request = new SearchRequest();
    request.indices("user");

    SearchSourceBuilder builder = new SearchSourceBuilder();

    AggregationBuilder aggregationBuilder = AggregationBuilders.terms("ageGroup").field("age");
    builder.aggregation(aggregationBuilder);

    request.source(builder);
    SearchResponse response = esClient.search(request, RequestOptions.DEFAULT);

    SearchHits hits = response.getHits();

    System.out.println(hits.getTotalHits());
    System.out.println(response.getTook());

    for ( SearchHit hit : hits ) {
        System.out.println(hit.getSourceAsString());
    }
```














