---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "MySQL的七种JOIN"
date: "2021-03-04"
categories: 
  - "Backend MYSQL"
---


## 建表

在这里我们先建立两张有外键关联的两张表：

```sql
    CREATE DATABASE db0206;
    USE db0206;
    CREATE TABLE `db0206`.`tbl_dept`(  
      `id` INT(11) NOT NULL AUTO_INCREMENT,
      `deptName` VARCHAR(30),
      `locAdd` VARCHAR(40),
      PRIMARY KEY (`id`)
    ) ENGINE=INNODB CHARSET=utf8;
    CREATE TABLE `db0206`.`tbl_emp`(  
      `id` INT(11) NOT NULL AUTO_INCREMENT,
      `name` VARCHAR(20),
      `deptId` INT(11),
      PRIMARY KEY (`id`),
      FOREIGN KEY (`deptId`) REFERENCES `db0206`.`tb_dept`(`id`)
    ) ENGINE=INNODB CHARSET=utf8;
    /*插入数据*/
    INSERT INTO tbl_dept(deptName,locAdd) VALUES('RD',11);
    INSERT INTO tbl_dept(deptName,locAdd) VALUES('HR',12);
    INSERT INTO tbl_dept(deptName,locAdd) VALUES('MK',13);
    INSERT INTO tbl_dept(deptName,locAdd) VALUES('MIS',14);
    INSERT INTO tbl_dept(deptName,locAdd) VALUES('FD',15);
    INSERT INTO tbl_emp(NAME,deptId) VALUES('z3',1);
    INSERT INTO tbl_emp(NAME,deptId) VALUES('z4',1);
    INSERT INTO tbl_emp(NAME,deptId) VALUES('z5',1);
    INSERT INTO tbl_emp(NAME,deptId) VALUES('w5',2);
    INSERT INTO tbl_emp(NAME,deptId) VALUES('w6',2);
    INSERT INTO tbl_emp(NAME,deptId) VALUES('s7',3);
    INSERT INTO tbl_emp(NAME,deptId) VALUES('s8',4);
```

## Venn图与SQL语句的编写以及查询结果

1. 内连接

内连接Venn图

![图片.png](https://raw.githubusercontent.com/zhengstar94/zhengstar94.github.io/main/docs/assets/img/2022/02/%E5%9B%BE%E7%89%87-1240ab5ec28f4c28828903a840ffac25.png)

执行的SQL语句以及执行的查询结果：

- 执行的SQL语句
```sql
select * from tbl_dept a inner join tbl_emp b on a.id=b.deptId;
```
- 查询结果
  {% include figure.liquid loading="eager" path="assets/img/2021/03/%E5%9B%BE%E7%89%87-a80c6703e36640c890b31229f163db3b.png" class="img-fluid rounded z-depth-1" zoomable=true width="50%"%}

2. 左外连接
   {% include figure.liquid loading="eager" path="assets/img/2021/03/%E5%9B%BE%E7%89%87-d22b051cdf3c42f589d55b78fc687c98.png" class="img-fluid rounded z-depth-1" zoomable=true width="50%"%}

执行的sql语句以及执行的查询结果：

- 执行的sql语句
```sql
select * from tbl_dept a left join tbl_emp b on a.id=b.deptId; 
```

- 查询结果
  {% include figure.liquid loading="eager" path="assets/img/2021/03/%E5%9B%BE%E7%89%87-63ca117826c4454d8e5b76ef425686fa.png" class="img-fluid rounded z-depth-1" zoomable=true width="50%"%}

3. 右外连接
   {% include figure.liquid loading="eager" path="assets/img/2021/03/%E5%9B%BE%E7%89%87-2750c7e1e5d24721a0179353e1385955.png" class="img-fluid rounded z-depth-1" zoomable=true width="50%"%}


执行的sql语句以及执行的查询结果：

- 执行的sql语句
```sql
select * from tbl_dept a right join tbl_emp b on a.id=b.deptId 
```

- 查询结果
  {% include figure.liquid loading="eager" path="assets/img/2021/03/%E5%9B%BE%E7%89%87-aed8bfe45d024316a11eb2772a52f23b.png" class="img-fluid rounded z-depth-1" zoomable=true width="50%"%}

