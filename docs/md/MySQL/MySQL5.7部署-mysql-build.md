
如果使用docker的话，不用这么麻烦

```shell
docker pull mysql
```

# Linux环境下安装MySQL

## 1. MySQL手动选择版本

1. 先查询系统下的mysql版本

```shell
yum list installed | grep mysql
```

2. 如果存在系统自带的mysql及依赖，则将其卸载

```shell
yum remove 包名
```

3. 进入[mysql官网](https://dev.mysql.com/downloads/)

![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2021/10/image-083183b26a53460194684375957d5bd8.png)

根据自己的系统进行选择

![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2021/10/image-0cbe4c1160544169af50b51276d602fd.png)

右键点击复制链接

![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2021/10/image-886ad95e7c0b4470811a3bf7e05983e0.png)

这就是最新的下载地址[下载链接](https://dev.mysql.com/get/mysql80-community-release-el7-3.noarch.rpm)

4. 进入系统下载rpm包并安装本地mysql源

下载rpm包
--no-check-certificate代表不检查证书

```shell
wget https://dev.mysql.com/get/mysql80-community-release-el7-3.noarch.rpm --no-check-certificate
```

安装MySQL源

```shell
yum localinstall mysql80-community-release-el7-3.noarch.rpm
```

通过 yum localinstall 安装mysql源，可以帮助我们解决本地rpm包的依赖问题。

5. 验证是否安装成功

```shell
yum repolist all | grep mysql
```

![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2021/10/image-2b9ef50be9be4971b2904ef4da5ee61d.png)


6. 默认是80，需要吧默认安装改成57版本

```shell
vim /etc/yum.repos.d/mysql-community.repo
```

把80改成0把57改成1注意看后缀不要改错了

![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2021/10/image-3d9782b5e16c472d9e12bfbf71ecff57.png)

再运行一遍查询命令发现已经修改好了

![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2021/10/image-fdbd31fd49c74a4cab50ee59a9f8a560.png)

7. 安装MySQL

```shell
yum install mysql-community-server
```

8. 启动MySQL

```shell
启动mysql：systemctl start mysqld.service
或         systemctl start mysqld
查看mysql状态：systemctl status mysqld.service
```

![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2021/10/image-2965471fb73f4b7b9370560803f12bef.png)

9. 设置开机启动

```shell
systemctl enable mysqld
```

10. 启动mysql

```shell
mysql -u root -p
```

在要求输入密码时，因为mysql 5.7的初始密码不是空，直接按回车键不能成功登录，因此需先找到初始密码，才能登录。

```shell
grep 'temporary password' /var/log/mysqld.log
```

再次登录输入初始密码
这里，我想设置新密码为 123456，但出现了报错，这里的报错是mysql的密码策略问题，输入命令：

```shell
show variables like 'validate_password%'
```

查看 mysql初始的密码策略，发现密码的最小长度为8，密码的验证强度等级为MEDIUM，可以修改一下密码策略：

```shell
设置密码的验证强度等级：set global validate_password_policy=LOW
设置密码的最小长度：set global validate_password_length=6
```

11. 修改密码

```shell
alter user roothome.php?mod=space&uid=485241 identified by '新密码'
```

12. 设置远程连接MySQL服务器

```language
use mysql;
```
![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2021/10/image-94baa1722ba74c3a9a771f06690a5702.png)

我们需要把root用户的host修改成%，这里我推荐使用SQL语句来修改，比较简单方便！


![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2021/10/image-5470c51d8aad46088d3043315ddff398.png)

13. 修改root用户的登录权限

```shell
update user set host = '%' where user = 'root';
```

![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2021/10/image-5408c6aeec3441ee80cc555ea16dfba8.png)

再查询一遍，修改成功了

![image.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2021/10/image-44c5d7a204ab4d8fb0cf35ebbf17e2fa.png)


14. 刷新权限 使当前操作立即生效，就大功告成了

```shell
flush privileges
```
再查询一遍，修改成功了
