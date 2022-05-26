# 系统接入

1. 自己项目上引入xxl-job-core的maven依赖

2. 在MySQL中执行/xxl-job/doc/db/tables_xxl_job.sql的SQL脚本

3. 从Gitee或GitHub下载xxl-job的源码，修改xxl-job-admin调度中心的数据库配置，启动xxl-job-admin项目。

4. 在自己项目上添加xxl-job相关的配置信息

5. 使用@XxlJob注解修饰方法编写定时任务的相关逻辑

![图片.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2022/02/%E5%9B%BE%E7%89%87-5f0b60aade7240899df1931f51c56cf2.png)
