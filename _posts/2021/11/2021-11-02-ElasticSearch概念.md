---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "ElasticSearch概念"
date: "2021-01-02"
categories: 
  - "Backend ElasticSearch"
---

# 核心概念

## 索引（Index）

- 一个索引就是一个拥有几分相似特征的文档集合
- 比如说，你可以有一个客户数据的索引，另一个产品目录的索引，还有一个订单数据的索引。一个索引由一个名字来标识（必须全部是小写字母），并且当我们要对这个索引中的文档进行索引、搜索、更新和删除的时候，都要使用到这个名字。
- 在一个集群中，可以定义任意多的索引。能搜索的数据必须索引，这样的好处是可以提高查询速度，比如：新华字典前面的目录就是索引的意思，目录可以提高查询速度。
- **Elasticsearch 索引的精髓：一切设计都是为了提高搜索的性能**

## 类型（Type）
- 在一个索引中，你可以定义一种或多种类型。
- 一个类型是你的索引的一个逻辑上的分类/分区，其语义完全由你来定。
- 通常，会为具有一组共同字段的文档定义一个类型。不同的版本，类型发生了不同的变化
  {% include figure.liquid loading="eager" path="assets/img/2021/11/image-448ea79dd11b4a8ebe33d1b3e9ab0467.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

## 文档（Document）
- 一个文档是一个可被索引的基础信息单元，也就是一条数据
- 比如：你可以拥有某一个客户的文档，某一个产品的一个文档，当然，也可以拥有某个订单的一个文档。
- 文档以 JSON（Javascript Object Notation）格式来表示，而 JSON 是一个到处存在的互联网数据交互格式。
- 在一个 index/type 里面，你可以存储任意多的文档。

## 字段（Field）
- 相当于是数据表的字段，对文档数据根据不同属性进行的分类标识。

## 映射（Mapping）
- mapping 是处理数据的方式和规则方面做一些限制，如：某个字段的数据类型、默认值、分析器、是否被索引等等。
- 这些都是映射里面可以设置的，其它就是处理 ES 里面数据的一些使用规则设置也叫做映射，按着最优规则处理数据对性能提高很大，因此才需要建立映射，并且需要思考如何建立映射才能对性能更好。

## 分片（Shards）

- 一个索引可以存储超出单个节点硬件限制的大量数据。比如，一个具有 10 亿文档数据的索引占据 1TB 的磁盘空间，而任一节点都可能没有这样大的磁盘空间。或者单个节点处理搜索请求，响应太慢。

- 为了解决这个问题，Elasticsearch 提供了将索引划分成多份的能力，每一份就称之为分片。当你创建一个索引的时候，你可以指定你想要的分片的数量。

- 每个分片本身也是一个功能完善并且独立的“索引”，这个“索引”可以被放置到集群中的任何节点上。

- 分片很重要，主要有两方面的原因：
> 1. 允许你水平分割 / 扩展你的内容容量。
> 2. 允许你在分片之上进行分布式的、并行的操作，进而提高性能/吞吐量。

- 至于一个分片怎样分布，它的文档怎样聚合和搜索请求，是完全由 Elasticsearch 管理的，对于作为用户来说，这些都是透明的，无需过分关心。

- 被混淆的概念是
> - 一个 ```Lucene 索引``` 我们在 ```Elasticsearch 称作 分片``` 。
> - 一个 ```Elasticsearch 索引``` 是 ```分片的集合```。

- 当 Elasticsearch 在索引中搜索的时候， 他发送查询到每一个属于索引的分片(Lucene 索引)，然后合并每个分片的结果到一个全局的结果集。

##  副本（Replicas）
- 在一个网络 / 云的环境里，失败随时都可能发生，在某个分片/节点突然掉线后，有一个故障转移机制是非常有用并且是强烈推荐的。

- 为此目的，Elasticsearch 允许你创建分片的一份或多份拷贝，这些拷贝叫做复制分片(副本)。

- 复制分片之所以重要，有两个主要原因：
> - 在分片/节点失败的情况下，提供了高可用性。因为这个原因，注意到复制分片从不与原/主要（original/primary）分片置于同一节点上是非常重要的。
> - 扩展你的搜索量/吞吐量，因为搜索可以在所有的副本上并行运行。

- 总之，每个索引可以被分成多个分片。一个索引也可以被复制 0 次（意思是没有复制）或多次。一旦复制了，每个索引就有了主分片（作为复制源的原来的分片）和复制分片（主分片的拷贝）之别。分片和复制的数量可以在索引创建的时候指定。在索引创建之后，可以在任何时候动态地改变复制的数量，但是事后不能改变分片的数量。

- 默认情况下，Elasticsearch 中的每个索引被分片 1 个主分片和 1 个复制，这意味着，如果你的集群中至少有两个节点，你的索引将会有 1 个主分片和另外 1 个复制分片（1 个完全拷贝），这样的话每个索引总共就有 2 个分片，我们需要根据索引需要确定分片个数。

## 分配（Allocation）
- 将分片分配给某个节点的过程，包括分配主分片或者副本。
- 如果是副本，还包含从主分片复制数据的过程。这个过程是由 master 节点完成的。

# 系统架构

{% include figure.liquid loading="eager" path="assets/img/2021/11/image-7de7c54d11d24fdf8da3748cf9bb74af.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

- 一个运行中的 Elasticsearch 实例称为一个节点，而集群是由一个或者多个拥有相同cluster.name 配置的节点组成， 它们共同承担数据和负载的压力。当有节点加入集群中或者从集群中移除节点时，集群将会重新平均分布所有的数据。
- 当一个节点被选举成为主节点时， 它将负责管理集群范围内的所有变更，例如增加、删除索引，或者增加、删除节点等。 而主节点并不需要涉及到文档级别的变更和搜索等操作，所以当集群只拥有一个主节点的情况下，即使流量的增加它也不会成为瓶颈。 任何节点都可以成为主节点。我们的示例集群就只有一个节点，所以它同时也成为了主节点。
- 作为用户，我们可以将请求发送到集群中的任何节点 ，包括主节点。 每个节点都知道任意文档所处的位置，并且能够将我们的请求直接转发到存储我们所需文档的节点。 无论我们将请求发送到哪个节点，它都能负责从各个包含我们所需文档的节点收集回数据，并将最终结果返回給客户端。 Elasticsearch 对这一切的管理都是透明的。

# 分布式集群

## 单节点集群
- 在包含一个空节点的集群内创建名为 users 的索引，为了演示目的，我们将分配 3 个主分片和一份副本（每个主分片拥有一个副本分片）

```json
{
    "settings":{
        "number_of_shards":3,
        "number_of_replicas":1
    }
}
```

{% include figure.liquid loading="eager" path="assets/img/2021/11/image-04205288f72a43858a0f35ccb0d9dfa3.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

- 我们的集群现在是拥有一个索引的单节点集群。所有 3 个主分片都被分配在 node-1 。
  {% include figure.liquid loading="eager" path="assets/img/2021/11/image-d1c98152af5240d7ac6e75c3ce2a4292.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

- 通过 ```elasticsearch-head 插件``` 查看集群情况

{% include figure.liquid loading="eager" path="assets/img/2021/11/image-632479ea10f64a29a51bc2caf8c3a97a.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

- 当前我们的集群是正常运行的，但是在硬件故障时有丢失数据的风险。

## 故障转移

- 当集群中只有一个节点在运行时，意味着会有一个单点故障问题——没有冗余。 幸运的是，我们只需再启动一个节点即可防止数据丢失。当你在同一台机器上启动了第二个节点时，只要它和第一个节点有同样的 cluster.name 配置，它就会自动发现集群并加入到其中。但是在不同机器上启动节点的时候，为了加入到同一集群，你需要配置一个可连接到的单播主机列表。之所以配置为使用单播发现，以防止节点无意中加入集群。只有在同一台机器上运行的节点才会自动组成集群。
- 如果启动了第二个节点，我们的集群将会拥有两个节点的集群 : 所有主分片和副本分片都已被分配
  {% include figure.liquid loading="eager" path="assets/img/2021/11/image-5066c1eec6ba442dbcd733799032e2ee.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

- 通过 elasticsearch-head 插件查看集群情况

{% include figure.liquid loading="eager" path="assets/img/2021/11/image-a620e88a28734d3da34969870465f451.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}


## 水平扩容

- 怎样为我们的正在增长中的应用程序按需扩容呢？当启动了第三个节点，我们的集群将会拥有三个节点的集群 : 为了分散负载而对分片进行重新分配

{% include figure.liquid loading="eager" path="assets/img/2021/11/image-ac06c3a4c9774aca943b748f03bf44bf.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

{% include figure.liquid loading="eager" path="assets/img/2021/11/image-29d53c7894cb427a9ce7027bb9ca2394.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

- **但是如果我们想要扩容超过 6 个节点怎么办呢？**
  - 主分片的数目在索引创建时就已经确定了下来。
  - 实际上，这个数目定义了这个索引能够存储 的最大数据量。（实际大小取决于你的数据、硬件和使用场景。） 但是，读操作——搜索和返回数据——可以同时被主分片 或 副本分片所处理，所以当你拥有越多的副本分片时，也将拥有越高的吞吐量。
  - 在运行中的集群上是可以动态调整副本分片数目的，我们可以按需伸缩集群。让我们把副本数从默认的 1 增加到 2

```json
{
	"number_of_replicas" : 2
}
```

{% include figure.liquid loading="eager" path="assets/img/2021/11/image-1eddd7f0e608405aba7d466634d14fb2.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

	- users 索引现在拥有 9 个分片：3 个主分片和 6 个副本分片。 这意味着我们可以将集群扩容到 9 个节点，每个节点上一个分片。相比原来 3 个节点时，集群搜索性能可以提升 3 倍。
	{% include figure.liquid loading="eager" path="assets/img/2021/11/image-d6f706fe13de464b88f05a4599b39d51.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

	- 当然，如果只是在相同节点数目的集群上增加更多的副本分片并不能提高性能，因为每个分片从节点上获得的资源会变少。 你需要增加更多的硬件资源来提升吞吐量。
	- 但是更多的副本分片数提高了数据冗余量：按照上面的节点配置，我们可以在失去 2 个节点的情况下不丢失任何数据。

## 应对故障

- 关闭第一个节点，这时集群的状态为:关闭了一个节点后的集群。
- 关闭的节点是一个主节点。而集群必须拥有一个主节点来保证正常工作，所以发生的第一件事情就是选举一个新的主节点： Node 2 。在我们关闭 Node 1 的同时也失去了主分片 1 和 2 ，并且在缺失主分片的时候索引也不能正常工作。 如果此时来检查集群的状况，我们看到的状态将会为 red ：不是所有主分片都在正常工作。

{% include figure.liquid loading="eager" path="assets/img/2021/11/image-8ffa5d30017f48028bc83ad28981251a.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

- 幸运的是，在其它节点上存在着这两个主分片的完整副本， 所以新的主节点立即将这些分片在 Node 2 和 Node 3 上对应的副本分片提升为主分片， 此时集群的状态将会为yellow。这个提升主分片的过程是瞬间发生的，如同按下一个开关一般。

# 路由计算
- 当索引一个文档的时候，文档会被存储到一个主分片中。 Elasticsearch 如何知道一个文档应该存放到哪个分片中呢？当我们创建文档时，它如何决定这个文档应当被存储在分片1 还是分片 2 中呢？首先这肯定不会是随机的，否则将来要获取文档的时候我们就不知道从何处寻找了。实际上，这个过程是根据下面这个公式决定的：

{% include figure.liquid loading="eager" path="assets/img/2021/11/image-3ec930a8acc14d718dd2b5c3d5e830f8.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

> - routing 是一个可变值，默认是文档的 _id ，也可以设置成一个自定义的值。
> - routing 通过 hash 函数生成一个数字，
> - 然后这个数字再除以 number_of_primary_shards （主分片的数量）后得到余数
> - 这个分布在 0 到 number_of_primary_shards-1 之间的余数，就是我们所寻求
    的文档所在分片的位置。

- 这就解释了为什么我们要在创建索引的时候就确定好主分片的数量 并且永远不会改变这个数量：因为如果数量变化了，那么所有之前路由的值都会无效，文档也再也找不到了。

- 所有的文档 API（ get 、 index 、 delete 、 bulk 、 update 以及 mget ）都接受一个叫做 routing 的路由参数 ，通过这个参数我们可以自定义文档到分片的映射。一个自定义的路由参数可以用来确保所有相关的文档——例如所有属于同一个用户的文档——都被存储到同一个分片中。

























