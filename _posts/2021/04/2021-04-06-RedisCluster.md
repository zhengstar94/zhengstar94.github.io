---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "RedisCluster"
date: "2021-04-06"
categories: 
  - "Backend Redis"
---


## 介绍
海量数据+高并发+高可用的场景。Redis cluster 支撑 N 个 Redis master node，每个 master node 都可以挂载多个 slave node。这样整个 Redis 就可以横向扩容了。如果你要支撑更大数据量的缓存，那就横向扩容更多的 master 节点，每个 master 节点就能存放更多的数据了。
- 自动将数据进行分片，每个master上放一部分数据
- 提供内置的高可用支持，部分master不可用，还可以可以继续工作的

在 Redis cluster 架构下，每个 Redis 要放开两个端口号，比如一个是 6379，另外一个就是 加 1w 的端口号，比如 16379。

16379 端口号是用来进行节点间通信的，也就是 cluster bus 的东西，cluster bus 的通信，用来进行故障检测、配置更新、故障转移授权。cluster bus 用了另外一种二进制的协议， gossip 协议，用于节点间进行高效的数据交换，占用更少的网络带宽和处理时间。

## 节点间的内部通信机制
### 基本通信原理

集群元数据的维护有两种方式：集中式、Gossip协议。Redis cluster采用Gossip协议进行。

gossio协议，所有节点都持有一份元数据，不同的节点出现了元数据的变更，就不断将原数据发送给其他的节点，让其他的节点也进行元数据变更。

- 优点：元数据更新比较分散，不是集中在同一个地方，更新请求会陆续打到所有的节点去更新，降低了压力。
- 缺点：元数据更新有延时，导致集群中的一些操作有一些滞后

> - 10000端口：每个节点都有一个专门用于节点间通信的端口，就是自己提供服务的端口号+10000，比如7001，那么用于节点间通信的就是170001端口。每个节点每隔一段时间都会往另外节点发送ping消息，同时其他几个节点接收到ping返回pong。
> - 交换的信息：信息包括故障信息，节点的增删，hash slot信息等

### gossip协议
包含多种信息，包含ping，pong，meet，fail等。
- meet：某个节点发送meet给新加入的节点，让新节点加入集群中，然后新节点就会开始与其他节点进行通信。
- ping：每个节点都会频繁给其他节点发送ping，其中包含自己的状态还有自己维护的集群元数据，互相通过ping交换元数据。
- pong：返回ping和meet，包含自己的状态喝其他信息，用于信息广播和更新。
- fail：某个节点判断另一个节点fail之后，就发送fail给其他节点，通知其他节点某个节点宕机。

## 分布式寻址算法
- hash算法
  来了一个 key，首先计算 hash 值，然后对节点数取模。然后打在不同的 master 节点上。一旦某一个 master 节点宕机，所有请求过来，都会基于最新的剩余 master 节点数去取模，尝试去取数据。这会导致大部分的请求过来，全部无法拿到有效的缓存，导致大量的流量涌入数据库。

- 一致性hash算法+虚拟节点
- Redis clusetr的hash slot算法
  Redis clusetr 有固定的16384个hash slot，对每个key计算CRC16值，然后对16384取模，可以获取key对应的hash slot。

Redis cluster中每个master都会持有部分slot，比如3个master，那么可能每个masetr持有5000多个hash slot。hash slot让node的增删很简单，增加一个master，就将其他master的hash slot移动部分过去；减少一个master，就将它的hash slot移动到其他的master上。移动hash slot成本非常低。

任何一台机器宕机，另外两个节点，不影响的。因为 key 找的是 hash slot，不是机器。

## Redis cluster高可用与主备切换原理
原理与哨兵是类似的

### 判断节点宕机
如果一个节点认为另一个节点宕机，那么就是pfail，主观当即。如果多节点都认为另一个节点当即，那么就是fail，客观宕机。




