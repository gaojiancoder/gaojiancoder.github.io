---
title: MySQL学习笔记6
top: false
cover: false
toc: true
mathjax: true
date: 2021-08-01 11:01:36
password:
summary:
tags: notes
categories: MySQL
---
# ——SQL的四种连接查询
### 内连接
inner join 或者join
```mysql
create table person(
id int,
name varchar(20),
cardId int);
```
```mysql
create table card(
id int,
name varchar(20)
);
```
```mysql
insert into card values(1,'饭卡');
insert into card values(2,'建行卡);
insert into card values(3,'农行卡');
insert into card values(4,'工商卡');
```
```mysql
insert into person values(1,' 张三',1);
insert into person values(2,'李四',3);
insert into person values(3,'王五',6);
```
```mysql
select*from person inner join card on person.cardId=card.id;
```
+------+--------+--------+------+-----------+
| id   | name   | cardId | id   | name      |
+------+--------+--------+------+-----------+
|    1 | 张三   |      1 |    1 | 饭卡      |
|    2 | 李四   |      3 |    3 | 农行卡    |
+------+--------+--------+------+-----------+
2 rows in set (0.00 sec)
内联查询就是两张表中的数据通过某个字段相对查询出相关记录数据
### 外连接
左连接left join 或者 left outer join
右连接right join 或 right outer join
完全外连接 full join 或full outer join——————mysql暂不支持
```mysql
select*from person left join card on person.cardId=card.id;
```
+------+--------+--------+------+-----------+
| id   | name   | cardId | id   | name      |
+------+--------+--------+------+-----------+
|    1 | 张三   |      1 |    1 | 饭卡      |
|    2 | 李四   |      3 |    3 | 农行卡    |
|    2 | 李四   |      3 |    3 | 农行卡    |
|    3 | 王五   |      6 | NULL | NULL      |
+------+--------+--------+------+-----------+
4 rows in set (0.00 sec)
会把左边表里的数据取出来右边表中的数据有就显示出来没有就null。right join 咋刚刚相反
```mysql
select*from person right join card on person.cardId=card.id;
```
+------+--------+--------+------+-----------+
| id   | name   | cardId | id   | name      |
+------+--------+--------+------+-----------+
|    1 | 张三   |      1 |    1 | 饭卡      |
| NULL | NULL   |   NULL |    2 | 建行卡    |
|    2 | 李四   |      3 |    3 | 农行卡    |
| NULL | NULL   |   NULL |    4 | 工商卡    |
|    2 | 李四   |      3 |    3 | 农行卡    |
+------+--------+--------+------+-----------+
5 rows in set (0.00 sec)
## MySQL事务
事务是一个最小的不可分割的工作单元，事务能够保证一个业务的完整性。为的就是多条语句能同时成功或者同时失败。
```mysql
select @@autocommit;
```
+--------------+
| @@autocommit |
+--------------+
|            1 |
+--------------+
1 row in set (0.00 sec)
默认事务开启的作用是什么？
当我们去执行一个sql语句的时候，效果会立即体现出来，且不能回滚
```mysql
create database bank;
```
创建数据库
```my
create table user(
id int primary key,
name varchar(20),
money int
);
```
```mysql
insert into user values(1,'a',1000);
```
```mysql
select*from user;
```
+----+------+-------+
| id | name | money |
+----+------+-------+
|  1 | a    |  1000 |
+----+------+-------+
rollback事务回滚（撤销上一句操作）
### 事务的四大特征
A  原子性：事务是最小的单位，不可分割
C 一致性：事务要求，同一事务中的sql语句，必须保证同时成功或者失败
I 隔离性：事务1和事务2之间具有隔离性
D  持久性：事务一旦结束，就不可回滚
### 总结
事务开启：
1修改默认提交 set autocommit = 0
2 begin；
3 start transaaction;
事务手动提交：commit；
事务手动回滚：rollback；
### 事务的隔离性
1,read uncommitted    读未提交的
2,read committed     读已经提交的
3,repeatable read     可以重复读
4,serializable         串行化
### 如何查看数据库隔离级别
MySQL 8.0 ：
select @@global.transaction_isolation;————系统级别的
select @@transaction_isolation;————会话级别的
MySQL5.x：
select @@global.tx_isolation;————系统级别的
select @@tx_isolation;————会话级别的
如何修改隔离级别
set global transaction isolation level 隔离级别;
### 隔离性解析
1—  会出现脏读  既a提交可以回滚的数据b在另一边看到了 之后a回滚
2—   会出现不可重复读  既a提交了不可回滚 b也提交 会出现时差性前后看到不一致
3—   a在一边提交 b在另一边看不见 但是如果提交相同的会出现错误提示已提交过来  这叫做幻读
4—   a开启事务 start transaaction;写入操作 ，b也开启事务写入操作 a不commit结束事务操作b就无法执行语句
多个人同时只能一人进行操作 其他人会进入排队状态（串行化）。  
—串行化带来的问题是性能差
