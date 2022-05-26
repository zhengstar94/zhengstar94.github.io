
# 附件预览功能(openoffice)


[官方下载](http://www.openoffice.org/download/index.html)



## Window安装

1. 双击安装包
	- 打开运行程序 
	![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2021/10/image-847778cbef584158bb02f239f164c585.png)
	- 点击下一步按钮 
	![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2021/10/image-8541ae593e584674859d7978e005e80e.png)
	- 点击浏览按钮 选择安装目录路径
	![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2021/10/image-91744f2a52484b73939126c10588234d.png)
	- 会自动检测系统中的插件,如果需要会自动安装
	![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2021/10/image-4eaf6d35a14c4865b8a41fd455232873.png)
	- 输入使用的用户,以及选择用户权限.点击下一步按钮
	![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2021/10/image-6d6b4e05d3104c198d1d94023cce5ead.png)
	- 勾选通常安装，点击下一步
	![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2021/10/image-db27f9f43b58438e9840d4c3c857b088.png)
	- 点击完成 即安装结束
	![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2021/10/image-65c59b83fcb545f8a10c5e9d8ec3a975.png)

2. 接下来以命令方式启动OpenOffice服务

**进入OpenOffice 4\program目录 执行第三段命令**


```linux
cd  C:\Program Files (x86)\OpenOffice 4\program  

soffice -headless -accept="socket,host=127.0.0.1,port=8100;urp;" -nofirststartwizard  
```

**服务打开成功之后在任务管理器可以看见soffice.bin的进程。**
![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2021/10/image-aec6440132724fbdbdf5573b99eb19e6.png)




## Linux环境下安装

1. 下载Apache_OpenOffice_4.1.6_Linux_x86-64_install-rpm_zh-CN.tar.gz

本人资源包放在 /opt/moudles 中, 解压后放在 /opt/softwares 中

2. 解压openoffice包

```linux
tar -zxvf Apache_OpenOffice_4.1.4_Linux_x86-64_install-rpm_zh-CN.tar.gz  /opt/softwares/  
```

3. 解压之后会在/opt/softwares中生成zh-CN文件夹，进入zh-CN中RPMS文件夹

```linux
cd /opt/softwares/zh-CN/RPMS  
```

![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2021/10/image-6a3c7a90523948b7a897e14e95b3e6e5.png)


4. 运行yum localinstall *.rpm

```linux
yum localinstall *.rpm  
```

5. 成功之后会在当前目录生成 desktop-integration文件夹，运行 yum localinstall 

![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2021/10/image-b6aa5a2522154e4e83fffb8211c418b1.png)

```linux
yum localinstall openoffice4.1.4-redhat-menus-4.1.4-9788.noarch.rpm 
```

6. 安装成功会在/opt目录下生成openoffice4文件夹
![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2021/10/image-b8b2f1a2e70245d0a1880a5c2eceabc7.png)

7. 启动(重点)

<font color='red'>启动服务和windows不同的是,需要在命令末尾加 & </font>

```linux
cd /opt/openoffice4/program  
soffice -headless -accept=""socket,host=127.0.0.1,port=8100;urp;"" -nofirststartwizard & 
```

查看是否启动成功

```linux
ps -ef|grep openoffice 
```

![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2021/10/image-5216c06b4aaa4e4fbb53c5051d82f1e1.png)

查看端口：

```linux
netstat -lnp |grep 8100
```

![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2021/10/image-dc8cd3c0fc40425c92a6d830cc337be6.png)

8. 完整命令

```linux
//进入目录  
cd /opt/openoffice4/program  
  
//启动  
soffice -headless -accept="socket,host=127.0.0.1,port=8100;urp;" -nofirststartwizard &  
  
  
//查看服务是否成功启动  
ps -ef|grep openoffice  
  
//查看端口  
netstat -lnp |grep 8100  

```

# 注意事项

> 使用openOffice一定要打开服务，重启系统的时候OpenOffice会关闭，需要手动启动，window系统启动看第二步，linux看启动步骤即可

