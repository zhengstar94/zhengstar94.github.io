# Windos10关闭端口占用进程

### 第一步：

```bash
netstat -ano | findstr 端口号
```

![图片.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2022/01/%E5%9B%BE%E7%89%87-1321e360bc7f4e57b31b9149521ac00e.png)


### 第二步：如果不想看是哪个进程占用的，就不用第二步(用来查看是什么程序占用这个端口)

```bash
tasklist | findstr 进程号
```

### 第三步：

```bash
taskkill -PID 进程号 -F 
```


