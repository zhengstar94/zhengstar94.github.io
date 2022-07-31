
# 单线程扩容

1. 初始情况
![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2021/08/image-e1e3f29d30e74cd7a8972bb0bbc81d19.png)


2. 开始扩容
![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2021/08/image-87403f4a7861400c8e0c0467c72555f9.png)

3. 最终结果
![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2021/08/image-2d5d172979bb4a2b973c43d6610013b9.png)


# 多线程扩容导致死循环
1. 两个线程A与B同时进行扩容
![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2021/08/image-d5cdefd11dc1416e8d538b777092900c.png)

2. A线程先进行扩容

![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2021/08/image-69958062bd0c49cc9121ce6dccfc7017.png)

**注意：此时若A线程挂起，开始执行B线程**
3. B开始第一次循环扩容
![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2021/08/image-61bc8d3a89c942bb92904ebd523ead69.png)

4.第二次循环

![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2021/08/image-77920c866b7340a89945f832b43dd7d3.png)

e2指向下一个元素，next指向下下个元素


5. 第三次循环
![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2021/08/image-8aae88a5728a4f36800f313317b6c152.png)

3需要指向下一个节点，即为2导致死循环





<font color=‘blue’ size='12px'>[原文参考链接](https://my.oschina.net/u/3694479/blog/3054837)</font>

<font color=‘blue’ size='5px'>[可参考链接](https://mp.weixin.qq.com/s/N8qIbQtovVsZbzdtjMJh2g)</font>