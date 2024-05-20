---
title: MySQL操作命令
tags:
  - 数据库
categories:
  - 数据库
comments: true
abbrlink: 61333
date: 2023-03-30 22:11:36
---

## MySQL

<!--more-->

1.进入数据库

`mysql -u 用户名 -P 端口号 -p 密码;`

2.展示数据库

`show database;`

3.创建数据库

`create database 数据库名称 default character set utf8;`

4.选中数据库

`use 数据库;`

5.创建表

`create table 表名(`

`属性 数据类型，`

`属性，数据类型`

`);`

6.删除表

`drop table 表名;`

7.查看表类型

`desc 表名;`

6.插入表数据

`insert into  表名``(属性1，属性2)` `values(属性1信息，属性2信息);`

7.查看表数据

`select * from tb_RiceCooker;`

8.更新表数据

`updata 表名 set 属性换成什么 where 哪个成员;`

9.删除表数据

`delete from 表名 where 属性;`

10.将表属性换一个名字

`select name ,age as Age ;`
