---
title: MySQL学习笔记4
top: false
cover: false
toc: true
mathjax: true
date: 2021-07-27 10:14:30
password:
summary:
tags: nodes
categories:
---
# 数据库的三大范式

## 第一范式 1NF
```
select*from student2;
```
+------+-----------+-----------------------------------------------+
| id   | name       | address                                                |
+------+-----------+-----------------------------------------------+
|    2 | 张胜男     | 中国黑龙江省哈尔滨市向阳区50号      |
|    3 | 张胜男     | 中国黑龙江省哈尔滨市武侯区90号      |
|    1 | 吴刚        | 中国黑龙江省哈尔滨市高新区100号    |
+------+-----------+-----------------------------------------------+
3 rows in set (0.00 sec)

这种还可以拆分的不属于第一范式

+----+-----------+--------+--------------+--------------+----------------+
| id | name      | cuntry | privence     | city              | details              |
+----+-----------+--------+--------------+--------------+----------------+
|  1 | 张三        | 中国   | 黑龙江省     | 哈尔滨市     | 向阳区50号     |
|  2 | 旺盛达    | 中国   | 黑龙江省     | 哈尔滨市     | 向阳区50号     |
|  3 | 张胜男    | 中国   | 黑龙江省     | 哈尔滨市     | 向阳区40号     |
+----+-----------+--------+--------------+--------------+----------------+
3 rows in set (0.00 sec)

达到不可拆分时为第一范式

## 第二范式2NF

必须满足第一范式前提下，2NF要求除主键外的每一列都必须完全依赖于主键。

如果出现不完全依赖只可能发生在联合主键的情况下。


### 示例——订单表
```
create table myorder(
product_id int,
customer_id int,
product_name varchar(20),
customer_name varchar(20),
primary key(product_id,customer_id)
);
```
mysql> desc myorder;
+---------------+-------------+------+-----+---------+-------+
| Field                   | Type            | Null  | Key   | Default | Extra |
+---------------+-------------+------+-----+---------+-------+
| product_id          | int                | NO   | PRI  | NULL    |          |
| customer_id       | int                | NO    | PRI  | NULL    |          |
| product_name    | varchar(20) | YES   |         | NULL    |           |
| customer_name | varchar(20) | YES   |         | NULL    |           |
+---------------+-------------+------+-----+---------+-------+
4 rows in set (0.00 sec)
不满足第二范式，需要进行拆表
```
create table myorder1(
Order_id int primary key,
product_id int,
Customer_id int
 );
```
mysql> desc myorder1;
+-------------+------+------+-----+---------+-------+
| Field              | Type | Null | Key | Default | Extra |
+-------------+------+------+-----+---------+-------+
| Order_id        | int  | NO    | PRI | NULL    |       |
| product_id     | int  | YES  |        | NULL    |       |
| Customer_id | int  | YES   |        | NULL    |       |
+-------------+------+------+-----+---------+-------+
3 rows in set (0.00 sec
```
create table product(
id int primary key,
Name varchar(20)
);
```
mysql> desc product;
+-------+-------------+------+-----+---------+-------+
| Field    | Type            | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| id         | int               | NO   | PRI | NULL    |       |
| Name  | varchar(20) | YES  |        | NULL    |       |
+-------+-------------+------+-----+---------+-------+
2 rows in set (0.00 sec)
```
create table customer(
Id int primary key,
Name varchar(20)
);
```
mysql> desc customer;;
+-------+-------------+------+-----+---------+-------+
| Field   | Type            | Null    | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| Id        | int               | NO     | PRI | NULL    |       |
| Name  | varchar(20) | YES   |        | NULL    |       |
+-------+-------------+------+-----+---------+-------+
2 rows in set (0.01 sec)
——分成三个表后，满足第二范式的设计

## 第三范式3NF

——必须先满足第二范式，除主键外不能有传递依赖关系。
### 什么是传递依赖
通俗的讲就是 a—>b,b—>c,a可以借着b推出c 这就是传递依赖

