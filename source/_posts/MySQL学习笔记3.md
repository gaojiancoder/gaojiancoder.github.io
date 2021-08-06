---
title: MySQL学习笔记3
top: false
cover: false
toc: true
mathjax: true
date: 2021-07-26 23:21:33
password:
summary:
tags: notes
categories: MySQL
---
# MySQL建表约束.sql

## 1.主键约束 primary key——主键不能重复不能为空

### 添加主键
alter table table-name add primary key(id,name);

### 删除主键
alter table table-name drop primary key;

### 设置双主键(联合主键)：只要加起来不重复就可以
create table user(id int,name varchar(20),password varchar(20),primary key(id,name));

## 2.自增约束 :auto-increment
起到管控id的作用，用在主键后面

## 3.外键约束   foreign key(class-id) references classes(id)

### 涉及到两个表：主表，副表。
```
create table students(
id int primary key,
name varchar(20),
class_id int,
foreign key(class_id) references classes(id));
```
Query OK, 0 rows affected (0.01 sec)

主表中的记录被副表引用是不可以被删除的。

主表中没有的数据值，在副表中不可以使用。

## 4.唯一约束 :alter table use1 add unique(name);

——约束修饰的字段的值不可以重复
```
alter table use1 add unique(name);
```
Query OK, 0 rows affected (0.00 sec)
Records: 0  Duplicates: 0  Warnings: 0
```
desc use1;
```
+-------+-------------+------+-----+---------+-------+
| Field    | Type            | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| id         | int               | NO   | PRI | NULL    |           |
| name  | varchar(20) | YES  | UNI | NULL    |           |
+-------+-------------+------+-----+---------+-------+
2 rows in set (0.00 sec)

### 如何删除唯一约束
Alter table table-name drop index <加了约束的字段>

## 5.非空约束 not null

## 6.默认约束
当我们没有插入字段值得时候，如果没有传值，就使用默认值
使用在每个字段的类型后面   ——default



