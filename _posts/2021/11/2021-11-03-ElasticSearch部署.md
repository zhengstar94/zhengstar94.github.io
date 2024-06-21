---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "ElasticSearch部署"
date: "2021-11-03"
categories: 
  - "Backend ElasticSearch"
---

# 1. Windows集群

## 部署集群
1.创建elasticsearch-cluster文件夹，在内部复制三个elasticsearch服务
   {% include figure.liquid loading="eager" path="assets/img/2021/11/image-9eaba4fba80649b9bd94065cb732cdfb.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}
2.修改集群文件目录中每个节点的 ```config/elasticsearch.yml``` 配置文件

- ```node-1001```节点

```yml
#节点 1 的配置信息：
#集群名称，节点之间要保持一致
cluster.name: my-elasticsearch
#节点名称，集群内要唯一
node.name: node-1001
node.master: true
node.data: true
#ip 地址
network.host: localhost
#http 端口
http.port: 1001
#tcp 监听端口
transport.tcp.port: 9301
#discovery.seed_hosts: ["localhost:9301", "localhost:9302","localhost:9303"]
#discovery.zen.fd.ping_timeout: 1m
#discovery.zen.fd.ping_retries: 5
#集群内的可以被选为主节点的节点列表
#cluster.initial_master_nodes: ["node-1", "node-2","node-3"]
#跨域配置
#action.destructive_requires_name: true
http.cors.enabled: true
http.cors.allow-origin: "*"
```

- ```node-1002```节点

```yml	
#节点 2 的配置信息：
#集群名称，节点之间要保持一致
cluster.name: my-elasticsearch
#节点名称，集群内要唯一
node.name: node-1002
node.master: true
node.data: true
#ip 地址
network.host: localhost
#http 端口
http.port: 1002
#tcp 监听端口
transport.tcp.port: 9302
discovery.seed_hosts: ["localhost:9301"]
discovery.zen.fd.ping_timeout: 1m
discovery.zen.fd.ping_retries: 5
#集群内的可以被选为主节点的节点列表
#cluster.initial_master_nodes: ["node-1", "node-2","node-3"]
#跨域配置
#action.destructive_requires_name: true
http.cors.enabled: true
http.cors.allow-origin: "*"
```

- ```node-1003```节点

```yml
#节点 3 的配置信息：
#集群名称，节点之间要保持一致
cluster.name: my-elasticsearch
#节点名称，集群内要唯一
node.name: node-1003
node.master: true
node.data: true
#ip 地址
network.host: localhost
#http 端口
http.port: 1003
#tcp 监听端口
transport.tcp.port: 9303
#候选主节点的地址，在开启服务后可以被选为主节点
discovery.seed_hosts: ["localhost:9301", "localhost:9302"]
discovery.zen.fd.ping_timeout: 1m
discovery.zen.fd.ping_retries: 5
#集群内的可以被选为主节点的节点列表
#cluster.initial_master_nodes: ["node-1", "node-2","node-3"]
#跨域配置
#action.destructive_requires_name: true
http.cors.enabled: true
http.cors.allow-origin: "*"
```

## 启动集群
1.启动前先删除每个节点中的 data 目录中所有内容（如果存在）
   {% include figure.liquid loading="eager" path="assets/img/2021/11/image-0739aa0e074548cda220affc6e3e44d4.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

2.分别双击执行 bin/elasticsearch.bat, 启动节点服务器，启动后，会自动加入指定名称的集群
{% include figure.liquid loading="eager" path="assets/img/2021/11/image-983536d47da24321a04e6c21b631da71.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

## 测试集群
1.使用 Postman 测试，查看集群状态
- node-1001 节点：http://127.0.0.1:1001/_cluster/health

{% include figure.liquid loading="eager" path="assets/img/2021/11/image-bc61e9384f8944fd94cc5e2ed9e8ca99.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

- node-1002 节点: http://127.0.0.1:1002/_cluster/health

{% include figure.liquid loading="eager" path="assets/img/2021/11/image-e16896b7a459475eb71178dfd5d213dc.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

- node-1003 节点: http://127.0.0.1:1003/_cluster/health

{% include figure.liquid loading="eager" path="assets/img/2021/11/image-bf2f6b58abe143a28ee05f2e243f56b6.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

- 其中：
> - status ：字段指示看当前集群在总体上是否工作正常。它的三种颜色含义如下:
> - green ：所有的主分片和副本分片都正常运行。
> - yellow ： 所有的主分片都正常运行，但不是所有的副本分片都正常运行。
> - red ： 有主分片没能正常运行。

2.向集群中的 node-1001 节点增加索引
   {% include figure.liquid loading="eager" path="assets/img/2021/11/image-52133bbab10b4357ae8051305527f5c0.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

3.向集群中的 node-1002 节点查询索引

{% include figure.liquid loading="eager" path="assets/img/2021/11/image-7658f57a801c4f1a99927e8c0097c95f.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

# 2. Linux 单机
## 软件下载
- [软件下载地址](https://www.elastic.co/cn/downloads/past-releases/elasticsearch-7-8-0)
  {% include figure.liquid loading="eager" path="assets/img/2021/11/image-a2bf3649e7434536971b11d5d09929f1.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

## 软件安装
1.解压软件，将下载的软件解压缩

```bash
# 解压缩
tar -zxvf elasticsearch-7.8.0-linux-x86_64.tar.gz -C /opt/module
# 改名
mv elasticsearch-7.8.0 es
```

2.创建用户
   因为安全问题，Elasticsearch 不允许 root 用户直接运行，所以要创建新用户
   在 root 用户中创建新用户

```linxu
useradd es #新增 es 用户
passwd es #为 es 用户设置密码
userdel -r es #如果错了，可以删除再加
chown -R es:es /opt/module/es #文件夹所有者
```
3.修改配置文件
- 修改 ```/opt/module/es/config/elasticsearch.yml``` 文件

```linxu
# 加入如下配置
cluster.name: elasticsearch
node.name: node-1
network.host: 0.0.0.0
http.port: 9200
cluster.initial_master_nodes: ["node-1"]
```

- 修改 ```/etc/security/limits.conf```

```linxu
# 在文件末尾中增加下面内容
# 每个进程可以打开的文件数的限制
es soft nofile 65536
es hard nofile 65536
```

- 修改 ```/etc/security/limits.d/20-nproc.conf```

```linxu
# 在文件末尾中增加下面内容
# 每个进程可以打开的文件数的限制
es soft nofile 65536
es hard nofile 65536
# 操作系统级别对每个用户创建的进程数的限制
* hard nproc 4096
# 注：* 带表 Linux 所有用户名称
```

- 修改 ```/etc/sysctl.conf```

```linxu
# 在文件中增加下面内容
# 一个进程可以拥有的 VMA(虚拟内存区域)的数量,默认值为 65536
vm.max_map_count=655360
```

- 重新加载

```bash
sysctl -p
```

4.启动软件，使用ES用户启动

```bash
cd /opt/module/es/
#启动
bin/elasticsearch
#后台启动
bin/elasticsearch -d
```

- 启动后，会动态生成文件，如果文件所属用户不匹配，会发生错误，需要重新进行修改用户和用户组
  {% include figure.liquid loading="eager" path="assets/img/2021/11/image-58c0e97e39fe4c8198a8bdab03e659df.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

- 关闭防火墙

```bash
#暂时关闭防火墙
systemctl stop firewalld
#永久关闭防火墙
systemctl enable firewalld.service #打开放货抢永久性生效，重启后不会复原
systemctl disable firewalld.service #关闭防火墙，永久性生效，重启后不会复原
```

## 测试软件
- 浏览器中输入地址：http://linux1:9200/
  {% include figure.liquid loading="eager" path="assets/img/2021/11/image-e6f758310595468e974792308293fda9.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}

# 3. Linux集群

## 软件下载
- [软件下载地址](https://www.elastic.co/cn/downloads/past-releases/elasticsearch-7-8-0)

{% include figure.liquid loading="eager" path="assets/img/2021/11/image-764cc3f0088c45d68a57252eec6fdb12.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}


## 软件安装

1.解压软件，将下载的软件解压缩

```bash
# 解压缩
tar -zxvf elasticsearch-7.8.0-linux-x86_64.tar.gz -C /opt/module
# 改名
mv elasticsearch-7.8.0 es-cluster
```
将软件分发到其他节点：linux2, linux3

2.创建用户

```bash
useradd es #新增 es 用户
passwd es #为 es 用户设置密码
userdel -r es #如果错了，可以删除再加
chown -R es:es /opt/module/es-cluster #文件夹所有者
```

3.修改配置文件，修改```/opt/module/es/config/elasticsearch.yml``` 文件，分发文件

```bash
# 加入如下配置
#集群名称
cluster.name: cluster-es
#节点名称，每个节点的名称不能重复
node.name: node-1
#ip 地址，每个节点的地址不能重复
network.host: linux1
#是不是有资格主节点
node.master: true
node.data: true
http.port: 9200
# head 插件需要这打开这两个配置
http.cors.allow-origin: "*"
http.cors.enabled: true
http.max_content_length: 200mb
#es7.x 之后新增的配置，初始化一个新的集群时需要此配置来选举 master
cluster.initial_master_nodes: ["node-1"]
#es7.x 之后新增的配置，节点发现
discovery.seed_hosts: ["linux1:9300","linux2:9300","linux3:9300"]
gateway.recover_after_nodes: 2
network.tcp.keep_alive: true
network.tcp.no_delay: true
transport.tcp.compress: true
#集群内同时启动的数据任务个数，默认是 2 个
cluster.routing.allocation.cluster_concurrent_rebalance: 16
#添加或删除节点及负载均衡时并发恢复的线程个数，默认 4 个
cluster.routing.allocation.node_concurrent_recoveries: 16
#初始化数据恢复时，并发恢复线程的个数，默认 4 个
cluster.routing.allocation.node_initial_primaries_recoveries: 16
```

- 修改 ```/etc/security/limits.conf``` ，分发文件

```bash
# 在文件末尾中增加下面内容
es soft nofile 65536
es hard nofile 65536
```

- 修改 ```/etc/security/limits.d/20-nproc.conf```，分发文件、

```bash
# 在文件末尾中增加下面内容
es soft nofile 65536
es hard nofile 65536
* hard nproc 4096
# 注：* 带表 Linux 所有用户名称
```

- 修改 ```/etc/sysctl.conf```

```bash
# 在文件中增加下面内容
vm.max_map_count=655360
```
- 重新加载

```linxu
sysctl -p
```

4.启动软件，分别在不同的节点上启动ES软件

```linxu
cd /opt/module/es-cluster
#启动
bin/elasticsearch
#后台启动
bin/elasticsearch -d
```

5.测试集群
   {% include figure.liquid loading="eager" path="assets/img/2021/11/image-93f53bea47884010bea2db9d76828b19.png" class="img-fluid rounded z-depth-1" zoomable=true width="70%"%}











