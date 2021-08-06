---
title: “MySQL如何在终端操作1”
top: false
cover: false
toc: true
mathjax: true
date: 2021-07-25 21:22:41
password:
summary:
tags: notes
categories: MySQL
---
# mysql关系型数据库

## 如何使用终端操作数据库?

### 1.如何登陆数据库

首先进入终端，p后面为登陆MySQL的密码 首次进入会提示设置，如果没有则不需用输入。
gaojian at gaojian in ~
$ mysql -uroot -p12345678

### 2.如何查询数据库服务器中所有的数据库？ show databases;

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| demo               |
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
5 rows in set (0.00 sec)

### 3.如何选中某一个数据库进行操作

如果没有选中数据库直接进行操作

mysql> select*from admin；
ERROR 1046 (3D000): No database selected
会报错

SQL语句 use demo
mysql> use demo
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql> select*from admin；
Empty set (0.00 sec)
使用use进入指定数据库

### 如何退出数据库服务器 exit;

mysql> exit;
Bye

### 如何在数据库服务器中创建我们的数据库？ Create database <name>;

Create database <name>;

mysql> create database test;
Query OK, 1 row affected (0.00 sec)

### 如何查看某个数据库中所有的数据表   show tables;

mysql> show tables;
+----------------+
| Tables_in_test |
+----------------+
| pet            |
+----------------+
1 row in set (0.00 sec)

### 如何创建一个数据表 create table pet

mysql> create table pet(
    -> name varchar(20),
    -> owner varchar(20),
    -> sex char(1),
    -> birth date,
    -> death date);
Query OK, 0 rows affected (0.01 sec)

### 查看创建好的数据表的结构 describe pet;

mysql> describe pet;
+-------+-------------+------+-----+---------+-------+
| Field    | Type     | Null | Key    | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| name  | varchar(20) | YES  |        | NULL    |       |
| owner | varchar(20) | YES  |        | NULL    |       |
| sex   | char(1)     | YES  |        | NULL    |       |
| birth | date        | YES  |        | NULL    |       |
| death | date        | YES  |        | NULL    |       |
+-------+-------------+------+-----+---------+-------+
5 rows in set (0.00 sec)

### 查看数据表中的记录 select*from pet;

mysql> select*from pet;
Empty set (0.01 sec)

### 如何往数据表中添加数据记录 insert into pet

mysql> insert into pet
    -> values('bob','gao','n','1999-3-2',null);
Query OK, 1 row affected (0.00 sec)

values输入的值要与表中的位置相对应

#### 再一次查询

mysql> select *from pet;
+------+-------+------+------------+-------+
| name | owner | sex  | birth            | death |
+------+-------+------+------------+-------+
| bob  | gao   | n      | 1999-03-02 | NULL  |
+------+-------+------+------------+-------+
1 row in set (0.00 sec)

#### 插入新数据和添加数据一样的操作

