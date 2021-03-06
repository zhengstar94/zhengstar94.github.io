<!-- docs/_sidebar.md -->

* [首页](README)

* 面试
  * [面试总结](md/Summary/总结.md)
      * [基础](md/Summary/基础.md)
      * [集合](md/Summary/集合.md)
      * [线程](md/Summary/线程.md)
      * [JVM](md/Summary/JVM.md)
      * [MySQL](md/Summary/MySQL.md)
      * [Redis](md/Summary/Redis.md)
      * [计算机基础](md/Summary/计算机基础.md)
      * [Spring](md/Summary/Spring.md)
      * [MQ](md/Summary/MQ.md)
      * [分布式事务](md/Summary/分布式事务.md)
      * [设计模式](md/Summary/设计模式.md)
      * [终面](md/Summary/终面.md)

* 知识点梳理
  * [知识点梳理](md/Summary/知识点整理.md)

* 后端
  * [基础](md/Base/基础-基础.md)
      * [注解与反射](md/Base/注解与反射-annotation.md)
      * [序列化与反序列化](md/Base/序列化与反序列化-xu-lie-hua-yu-fan-xu-lie-hua.md)
      * [Java8Stream流递归树遍历](md/Base/Java8%20Stream流递归解决树形遍历-java8-stream-recursion-tree.md)
    
  * [集合](md/Collection/Collection.md)
    * [ArrayList与LinkedList](md/Collection/ArrayList%20与%20LinkedList-arraylist-linkedlist.md)
    * [HashMap](md/Collection/HashMap-hashmap.md)
    * [HashMap1.7造成死循环的原因](md/Collection/HashMap%201.7造成死循环的原因-hashmap-dead-loop.md)
    * [ConcurrentHashMap](md/Collection/ConcurrentHashMap-concurrenthashmap.md)

  * [数据库](md/MySQL/MySQL数据库-mysql.md)
    * [MySQL5.7部署](md/MySQL/MySQL5.7部署-mysql-build.md)
    * [一条sql语句在Mysql中如何被执行的？](md/MySQL/一条sql语句在Mysql中如何被执行的？-mysql-how-to-execute.md)
    * [MySQL的七种JOIN](md/MySQL/MySQL的七种JOIN-mysql的七种join.md)
    * [MySQL查找出重复的记录](md/MySQL/MySQL查找出重复的记录-mysql-same-record.md)
    * [MYSQL优化建议](md/MySQL/MYSQL优化建议-sql-optimization.md)
    * [MVCC多版本并发控制](md/MySQL/MVCC多版本并发控制-mvcc.md)
    * [MySQL的主从复制](md/MySQL/MySQL的主从复制-mysql-master-slave-copy.md)
    * [分库分表](md/MySQL/分库分表-separate-database-table.md)
    * [分库分表实战](md/MySQL/分库分表实战-separate-db-table-experiance.md)
    * [分库分表做到永不迁移数据和避免热点](md/MySQL/分库分表：做到永不迁移数据和避免热点-分库分表做到永不迁移数据和避免热点.md)
    * [数据库的读写分离](md/MySQL/数据库的读写分离-read-write-separate.md)
    * [Mysql实战常见问题](md/MySQL/Mysql实战常见问题-mysql-question-1.md)
    * [MySQL服务占用cpu100%，如何排查问题](md/MySQL/MySQL%20服务占用cpu%20100%25，如何排查问题%3F%20-mysql-cpu-100.md)
    
  * [Reids](md/Redis/Redis.md)
      * [Redis基础](md/Redis/Redis-redis.md)
      * [Redis数据类型](md/Redis/Redis的基本数据类型-redis-data-type.md)
      * [Redis 如何做到高可用](md/Redis/Redis如何做到高可用-redis高可用.md)
      * [Redis 主从架构](md/Redis/Redis主从架构-redis主从架构.md)
      * [Redis 集群解决方案](md/Redis/Redis集群解决方案-redis-cluster-scheme.md)
      * [Redis cluster](md/Redis/Redis%20cluster-rediscluster.md)
      * [Redis 哈希槽实战](md/Redis/Redis%20哈希槽实战-redis-hashslot-experience.md)
      * [Redis实现分布式锁](md/Redis/Redis实现的分布式锁-distributed-lock-redis.md)
      * [Redis实现分布式锁part2](md/Redis/分布式锁的实现之%20redis%20篇-distributed-lock-redis-part2.md)
      * [Redis实现分布式锁与Redisson](md/Redis/Redis实现分布式锁与Redisson-redis-separate-lock.md)
      * [分布式锁对比](md/Redis/分布式锁对比-distributed-lock-compare.md)
      * [布隆过滤器](md/Redis/布隆过滤器的实现-bloomfilter.md)
      * [跳表](md/Redis/跳表-跳表.md)
      * [Redis工具类](md/Redis/Redis工具类-redisutil.md)
    
  * [线程](md/Thread/多线程.md)  
      * [线程池](md/Thread/线程池-线程池.md)
      * [线程池业务中的实践](md/Thread/线程池业务中的实践-线程池业务中的实践.md)
      * [锁的分类](md/Thread/锁的分类-lock-partition.md)
      * [ThreadLocal](md/Thread/ThreadLocal-threadlocal.md)
      * [Volatile](md/Thread/Volatile-volatile.md)
      * [Sychronized](md/Thread/Sychronized-synchronized.md)
      * [ReentrantLock](md/Thread/ReentrantLock-reentrantlock-clh.md)
      * [AQS抽象队列同步器](md/Thread/AQS(抽象队列同步器)-aqs.md)
      * [AQS源码分析](md/Thread/AQS源码分析-aqs源码分析.md)
      * [CountDownLatch](md/Thread/CountDownLatch-countdownlatch.md)
    
  * [计算机基础](md/ComputerBase/计算机网络-计算机网络.md)
    * [TCP的三次握手与四次挥手](md/ComputerBase/TCP的三次握手与四次挥手-tcp-connection.md)
    * [BIO、NIO、AIO](md/ComputerBase/IO模型之BIO、NIO、AIO-io模型.md)

  * [JVM](md/JVM/JVM与垃圾回收-jvm.md)
    * [JVM类加载与双亲委派](md/JVM/JVM%20之%20类加载与双亲委派-jvm之类加载与双亲委派.md)
    * [线上问题排查思路](md/JVM/Java%20应用线上问题排查思路、常用工具小结-java-porblem-sort-out.md)
    * [JVM调优工具与调优实战](md/JVM/JVM调优工具与调优实战-jvm-optimize.md)

  * [Spring](md/Spring/Spring-spring.md)
    * [Spring IOC启动过程](md/Spring/Spring%20IOC启动过程-spring-ioc-start.md)
    * [Spring AOP](md/Spring/Spring%20AOP-spring-aop.md)
    * [Java手写实现AOP](md/Spring/Java手写实现AOP-java-handwrite-aop.md)
    * [Spring的生命周期](md/Spring/Spring的生命周期-spring-bean-life.md)
    * [Spring循环依赖](md/Spring/Spring循环依赖-spring-depend.md)
    * [SpringBoot](md/Spring/Spring%20Boot-springboot.md)
    * [SpringBoot自动装配](md/Spring/Spring%20Boot自动装配-spring-auto-configur.md)
    * [SpringBoot自动装配原理](md/Spring/spring%20boot自动装配原理-springboot自动装配原理.md)
    * [Spring与Redis通信设计结构图](md/Spring/Spring与Redis通信设计结构图-spring-redis-call.md)
    * [搭建一个SpringCloud项目](md/Spring/搭建一个Spring%20Cloud项目-spring-cloud-project-build.md)

  * [MQ](md/MQ/消息MQ-mq.md)
    * [消息的顺序性](md/MQ/消息的顺序性-消息的顺序性.md)
    * [如何保证消息的幂等性](md/MQ/如何保证消息的幂等性-如何保证消息的幂等性.md)
    * [如何保证消息队列的高可用](md/MQ/如何保证消息队列的高可用-如何保证消息队列的高可用.md)
    * [如何处理消息丢失的问题](md/MQ/如何处理消息丢失的问题(如何保证消息的可靠性传输)-如何处理消息丢失的问题.md)
    * [如何解决消息队列的延时以及过期失效问题](md/MQ/如何解决消息队列的延时以及过期失效问题？消息队列满了以后该怎么处理？有几百万消息持续积压几小时，说说怎么解决？-mq-invalid.md)

  * [ElasticSearch](md/ES/ElasticSearch-elasticsearch-base-knowledge.md)
    * [ElasticSearch相关概念](md/ES/ElasticSearch相关概念-elasticsearch-conception.md)
    * [ElasticSearch部署](md/ES/ElasticSearch部署-elasticsearch-build.md)

  * [分布式](md/Distributed/分布式事务解决方案part%201-distribute-work-solve.md)
    * [分布式事务解决方案](md/Distributed/分布式事务解决方案part%202-distribute-transaction-solution.md)
    * [基于rocketmq的分布式事务](md/Distributed/基于rocketmq的分布式事务-rocketmq.md)
    * [TCC实例](md/Distributed/TCC实例-tcc.md)
    * [Zookeeper实现分布式锁](md/Distributed/Zookeeper实现分布式锁-zookeeper.md)
    * [分布式系统接口，如何避免表单的重复提交](md/Distributed/分布式系统接口，如何避免表单的重复提交-distributed-idempotent.md)

  * [工作记录](md/Work/工作内容记录.md)
    * [Linux环境下安装svn](md/Work/Linux环境下安装svn-linux-svn.md)
    * [js上精度丢失解决方案](md/Work/js上精度丢失解决方案-js-degree-lost.md)
    * [windos10关闭端口占用进程](md/Work/windos10关闭端口占用进程-windos10关闭端口占用进程.md)
    * [如何高效的使用Git](md/Work/如何高效的使用%20Git-git.md)
    * [本地jar引入到maven方](md/Work/本地jar引入到maven方式-local-jar-to-maven.md)
    * [表驱动法](md/Work/表驱动法-table-mover.md)
    * [防止xss攻击](md/Work/防止xss攻击-xss.md)
    * [附件预览功能(openoffice)](md/Work/附件预览功能(openoffice)-file-preview.md)
    * [输入URL并回车后，如果页面迟迟没有出现，怎么去排查问题？](md/Work/输入URL并回车后，如果页面迟迟没有出现，怎么去排查问题？.md)
    * [JWT基础](md/Work/JWT基础.md)

  * [系统设计案例](md/SystemDesign/系统设计相关.md)
    * [如何统计网站UV？](md/SystemDesign/「系统设计面试题」如何统计网站UV？-system-design-uv.md)
    * [如何解决网站大文件上传问题？](md/SystemDesign/「系统设计面试题」如何解决网站大文件上传问题？-system-design-file-download.md)
    * [如何设计一个短链系统？](md/SystemDesign/「系统设计面试题」如何设计一个短链系统？-system-design-short-link.md)
    * [如何设计一个秒杀系统？](md/SystemDesign/「系统设计面试题」如何设计一个秒杀系统？-system-design-flash-sale.md)
    * [如何设计一个站内消息系统？](md/SystemDesign/「系统设计面试题」如何设计一个站内消息系统？-system-design-station-message.md)
    * [XXL_JOB(分布式定时任务框架)与系统的接入](md/SystemDesign/XXL_JOB(分布式定时任务框架)与系统的接入-xxljob分布式定时任务框架与系统的接入.md)
    * [图片验证码功能](md/SystemDesign/图片验证码功能-img-verify.md)
    * [如何设计一个高并发场景？](md/SystemDesign/如何设计一个高并发场景-high-concrruent-design.md)
    * [如何高效对接第三方支付？](md/SystemDesign/如何高效对接第三方支付-intergrate-payment.md)
    * [日志的使用？](md/SystemDesign/日志的使用-use-log.md)
    * [短信验证服务(阿里云)](md/SystemDesign/短信验证服务(阿里云)-phone-message.md)
    * [订单主动过期释放，如何实现？](md/SystemDesign/订单主动过期释放，如何实现？-order-to-expire.md)
    * [手写LRU](md/SystemDesign/手写LRU-hand-lru.md)
    * [如何设计好一个接口](md/SystemDesign/如何设计好一个接口.md)
    * [架构设计之微服务拆分](md/SystemDesign/架构设计之微服务拆分.md)
    * [写一个并行调用模板](md/SystemDesign/写一个并行调用模板.md)
    
  * [数据结构预算法](md/Algorithm/数据结构与算法-数据结构与算法.md)
    * [排序算法](md/Algorithm/排序算法-排序算法.md)
    * [二分查找part1](md/Algorithm/Binary%20Search%20二分查找-binary-search.md)
    * [二分查找part2](md/Algorithm/binary_search-binarysearch.md)
    * [一致性算法](md/Algorithm/一致性Hash算法-一致性hash算法.md)
    * [限流算法](md/Algorithm/限流算法-limiter-algorithm.md)
    
  * [设计模式](md/Design/面向对象设计原则-object-oriented-design-principles.md)
    * [面向对象设计过程](md/Design/面向对象的设计过程-face-to-object-design.md)
    * [UML类图](md/Design/UML类图-uml-lei-tu.md)
    * [结构型模式](md/Design/结构型模式-结构型模式.md)
    * [创建型模式](md/Design/创建型模式-creational-pattern.md)
    * [工厂方法模式](md/Design/工厂方法模式(Factory%20Method%20Pattern)-factory-method-pattern.md)
    * [简单工厂](md/Design/简单工厂模式(%20Simple%20Factory%20Pattern%20)-simple-factory-pattern.md)
    * [抽象工厂模式](md/Design/抽象工厂模式(Abstract%20Factory)-abstract-factory.md)
    * [建造者模式](md/Design/建造者模式（Builder）-builder.md)
    * [适配器模式](md/Design/适配器模式(Adapter)-适配器模式adapter.md)
    * [单例模式](md/Design/单例模式（Singleton）-singleton.md)
    * [接口分离原则](md/Design/接口分离原则（Interface%20Segregation%20Principle）-interface-segregation-principle.md)

* 网络
  * [为什么TCP建立连接需要三次握手](md/Network/为什么TCP建立连接需要三次握手.md)

* Linux
  * [Linux1](md/Linux/LinuxDay1.md)
  * [Linux2](md/Linux/LinuxDay2.md)
  * [Linux3](md/Linux/LinuxDay3.md)

* 学习方式
  * [如何做编程知识投资及减少知识失效的影响](md/Study/如何做编程知识投资及减少知识失效的影响.md)
  * [10x程序员工作法](md/Study/10x程序员工作法.md)
  * [通过整合学习资料被动收入](md/Study/通过整合学习资料全流程自动化，一个月被动收入.md)
  * [从零开始做了一个养活自己的公众号](md/Study/从零开始做了一个养活自己的公众号.md)
  * [深入剖析Java新特性](md/Study/深入剖析Java新特性.md)
  * [面试学习经验总结](md/Study/面试学习经验总结.md)
  * [湾区大厂程序员薪水构成,level和对应期望](md/Study/湾区大厂程序员薪水构成,level和对应期望.md)
  * [程序员如何把控自己的职业](md/Study/程序员如何把控自己的职业.md)
  * [加班与效率](md/Study/加班与效率.md)
  * [技术人员的发展之路](md/Study/技术人员的发展之路.md)

* 英语
  * [Interview](md/English/interview.md)
  * [英语听说](md/English/英语听说.md)
  * [英语学习](md/English/英语学习.md)
  * [全英文面试如何面对](md/English/求职遭遇全英文面试，我靠这些方法度过难关.md)
  * [敏捷Scrum](md/English/敏捷Scrum.md)
    
* 生活
  * [FIRE](md/Life/FIRE-2021-08-19-12-36-20.md)
  * [低碳](md/Life/低碳饮食-diet.md)
  * [减肥篇](md/Life/lost-weight.md)
  * [延寿指南](md/Life/延寿指南.md)
  * [通用国内旅行攻略模板](md/Life/通用国内旅行攻略模板.md)

* 出国
  * [德国欧盟蓝卡(工作签证)办理流程](md/Abroad/德国欧盟蓝卡(工作签证)办理流程.md)
  * [德国学历波恩认证(ZAB)流程](md/Abroad/德国学历波恩认证(ZAB)流程.md)

* 面经
  * [面试经验分享Part1](md/Ready/面试经验分享Part1.md)
  * [BQ准备与练习](md/Ready/BQ.md)

* interview
  * [项目经验](md/Interview/projectExperience.md)

* python
  * [Python笔记](md/Python/Python笔记.md)

<!-- * 英语学习计划
  * [第一天](md/another/learn01.md) -->

<!-- * 30天英语学习计划
  * [Plan](md/tiffani/30daysPlan.md)
  * [Day1](md/tiffani/day1.md) -->