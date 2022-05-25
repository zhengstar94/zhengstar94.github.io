
# 介绍
一致性哈希算法目的是解决分布式缓存问题。在移除或者添加一个服务器的时候，能够尽可能小的改变已存在的服务请求与处理服务器之间的映射关系。一致性哈希解决了简单哈希算法在分布式哈希表中存在的动态伸缩等问题。


## 详解
一致性哈希算法是将所有的哈希值构成一个环，范围在```0～2^32-1```
![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2021/09/image-223af0f2e6604875ae1f41cf2daa968b.png)
之后将各个节点散列到这个环上，可以用节点的IP等唯一字段作为key进行hash散列
![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2021/09/image-14584761fdc44b4eae22b7a0406e557a.png)

之后需要将数据定位到对应的节点上，使用hash函数将key映射到这个环上
![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2021/09/image-6729ef8d4419410a93775654abe88f88.png)
这样按照顺时针方向就可以把k1定位到N1节点，k2定位到N3节点，k3定位到N2节点

**容错性**

假设这时N1宕机

![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2021/09/image-d9d410640e4a427ba608bb92f9484b02.png)


根据顺时针方向，k2与k3保持不变，只有k1重新被映射到N3。保证了容错性，当一个节点宕机时只会影响到少部分的数据

**扩展性**

当新增一个节点时：
![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2021/09/image-224e11ce31934763b30f8397c2f454a3.png)

在N2与N3之间新增一个节点N4，这时会发现受影响的数据只有k3，其余数据保持不变。保证了扩展性。


**到目前为止该算法依旧有些问题，当节点比较少的时候容易出现数据不均匀的情况**
![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2021/09/image-4703fbad9c784893b6272b30dddf9165.png)

这样会导致大部分数据都在N1，少量数据在N2


所以一致性哈希算法引入虚拟节点，**每一个节点再进行多次hash，生成多个节点放置在环上称为虚拟节点**。
![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2021/09/image-665ea827532d4c50b1afaa3442ed0f60.png)

计算的时候可以再IP后面加上编号生成hash值。

这样只需要在原有的基础上多一步由虚拟节点映射到实际节点的步骤可以让少量节点满足均匀性。




