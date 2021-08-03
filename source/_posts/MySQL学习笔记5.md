---
title: MySQL学习笔记5
top: false
cover: false
toc: true
mathjax: true
date: 2021-08-01 10:13:40
password:
summary:
tags: notes
categories:
---
# MySQL查询练习
### 学生表Student
——学号，姓名，性别，出生年月日，所在班级。
```mysql
create table student(
  sno varchar(20) primary key,
  sname varchar(20) not null,
  ssex varchar(10) not null,
  sbirthday datetime,
  class varchar(20)
  );
```
### ——课程表Course
——课程号，课程名称，教师编号
```mysql
create table course(
  cno varchar(20) primary key,
  sname varchar(20) not null,
  tno varchar(20) not null,
  foreign key(tno) references teacher(tno)
);
```
### ——成绩表 Score
——学号，课程号，成绩
```mysql
create table score(
  sno varchar(20) not null,
  cno varchar(20) not null,
  degree decimal,
  foreign key(sno) references student(sno),
     foreign key(cno) references course(cno),
  primary key(sno,cno)
  );
```
### ——教师表 Teacher
——教师编号，教师姓名，教师性别，出生年月日，职称，所在部门。
```mysql
create table teacher(
  tno varchar(20) primary key,
  tname varchar(20) not null,
  tsex varchar(10) not null,
  tbirthday datetime,
  prof varchar(20) not null,
  depart varchar(20) not null
  );
```
### ——往表中添加数据
—添加学生信息
—添加教师信息
—添加课程表
—添加成绩表
## 查询练习
### ——查询student表的所有记录。*代表所有
mysql> select *from student;
+-----+--------------+------+---------------------+-------+
| sno | sname        | ssex | sbirthday           | class |
+-----+--------------+------+---------------------+-------+
| 101 | 撒旦法达     | 男   | 1977-04-23 00:00:00 | 95037 |
| 102 | 阿斯蒂芬     | 女   | 1977-04-23 00:00:00 | 95033 |
| 103 | 发电房芬     | 女   | 1977-04-23 00:00:00 | 95035 |
| 104 | 曾阿凡达     | 男   | 1977-04-23 00:00:00 | 95033 |
| 105 | 四方达狗     | 男   | 1977-04-23 00:00:00 | 95032 |
| 106 | 曾华         | 女   | 1977-04-23 00:00:00 | 95031 |
| 107 | 阿法狗       | 男   | 1977-04-23 00:00:00 | 95033 |
| 108 | 沙发         | 男   | 1977-04-23 00:00:00 | 95033 |
| 109 | 曾曾华       | 女   | 1977-04-23 00:00:00 | 95033 |
+-----+--------------+------+---------------------+-------+
9 rows in set (0.00 sec)
### ——查询student表中的所有记录的sname，ssex，class列     select
mysql> select sname,ssex,class from student;
+--------------+------+-------+
| sname        | ssex | class |
+--------------+------+-------+
| 撒旦法达     | 男   | 95037 |
| 阿斯蒂芬     | 女   | 95033 |
| 发电房芬     | 女   | 95035 |
| 曾阿凡达     | 男   | 95033 |
| 四方达狗     | 男   | 95032 |
| 曾华         | 女   | 95031 |
| 阿法狗       | 男   | 95033 |
| 沙发         | 男   | 95033 |
| 曾曾华       | 女   | 95033 |
+--------------+------+-------+
9 rows in set (0.00 sec)
### ——查询教师所有的单位既不重复的depart列   select distinct 排重
mysql> select distinct depart from teacher;
+-----------------+
| depart          |
+-----------------+
| 计算机系        |
| 电子工程系      |
+-----------------+
2 rows in set (0.00 sec)
### ——查询score表中成绩在60-80之间的所有记录     查询区间 between。。。and
mysql> select*from score where degree between 60 and 80;
+-----+-------+--------+
| sno | cno   | degree |
+-----+-------+--------+
| 102 | 2-801 |     77 |
| 105 | 2-801 |     67 |
+-----+-------+--------+
2 rows in set (0.00 sec)
### ——查询score表中成绩为98，87的记录      in
select *from score where degree in (98,87);
mysql> select *from score where degree in (98,87);
+-----+-------+--------+
| sno | cno   | degree |
+-----+-------+--------+
| 103 | 3-802 |     87 |
| 104 | 2-802 |     87 |
| 106 | 3-801 |     98 |
| 108 | 3-802 |     87 |
+-----+-------+--------+
4 rows in set (0.00 sec)
### ——查询student表中95031班的性别为女的同学记录   or
mysql> select *from student where class='95033' or ssex = '女';
+-----+--------------+------+---------------------+-------+
| sno | sname        | ssex | sbirthday           | class |
+-----+--------------+------+---------------------+-------+
| 102 | 阿斯蒂芬     | 女   | 1977-04-23 00:00:00 | 95033 |
| 103 | 发电房芬     | 女   | 1977-04-23 00:00:00 | 95035 |
| 104 | 曾阿凡达     | 男   | 1977-04-23 00:00:00 | 95033 |
| 106 | 曾华         | 女   | 1977-04-23 00:00:00 | 95031 |
| 107 | 阿法狗       | 男   | 1977-04-23 00:00:00 | 95033 |
| 108 | 沙发         | 男   | 1977-04-23 00:00:00 | 95033 |
| 109 | 曾曾华       | 女   | 1977-04-23 00:00:00 | 95033 |
+-----+--------------+------+---------------------+-------+
7 rows in set (0.00 sec)
### ——以class降序查询student表的所有记录  order by ...desc
mysql> select *from student order by class desc;
+-----+--------------+------+---------------------+-------+
| sno | sname        | ssex | sbirthday           | class |
+-----+--------------+------+---------------------+-------+
| 101 | 撒旦法达     | 男   | 1977-04-23 00:00:00 | 95037 |
| 103 | 发电房芬     | 女   | 1977-04-23 00:00:00 | 95035 |
| 102 | 阿斯蒂芬     | 女   | 1977-04-23 00:00:00 | 95033 |
| 104 | 曾阿凡达     | 男   | 1977-04-23 00:00:00 | 95033 |
| 107 | 阿法狗       | 男   | 1977-04-23 00:00:00 | 95033 |
| 108 | 沙发         | 男   | 1977-04-23 00:00:00 | 95033 |
| 109 | 曾曾华       | 女   | 1977-04-23 00:00:00 | 95033 |
| 105 | 四方达狗     | 男   | 1977-04-23 00:00:00 | 95032 |
| 106 | 曾华         | 女   | 1977-04-23 00:00:00 | 95031 |
+-----+--------------+------+---------------------+-------+
9 rows in set (0.00 sec)
### ——查询以cno升序，degree降序查询score表的所有记录
mysql> select *from score order by cno asc,degree desc;
+-----+-------+--------+
| sno | cno   | degree |
+-----+-------+--------+
| 102 | 2-801 |     77 |
| 105 | 2-801 |     67 |
| 104 | 2-802 |     87 |
| 106 | 3-801 |     98 |
| 101 | 3-801 |     89 |
| 103 | 3-802 |     87 |
| 108 | 3-802 |     87 |
+-----+-------+--------+
7 rows in set (0.00 sec)
### ——查询95033班的学生人数   count (统计)
mysql> select count(*) from student where class = '95031';
+----------+
| count(*) |
+----------+
|        1 |
+----------+
1 row in set (0.00 sec)
### ——查询score表中的最高分的学生的学号和课程号（子查询或者排序）
mysql> select sno,cno from score where degree =(select max(degree) from score);
+-----+-------+
| sno | cno   |
+-----+-------+
| 106 | 3-801 |
+-----+-------+
1 row in set (0.01 sec)
### ——查询每门课的平均成绩  ——avg()平均  group by 分组
mysql> Select  cno,avg(degree) from score group by cno;
+-------+-------------+
| cno   | avg(degree) |
+-------+-------------+
| 2-801 |     72.0000 |
| 2-802 |     87.0000 |
| 3-801 |     93.5000 |
| 3-802 |     87.0000 |
+-------+-------------+
4 rows in set (0.00 sec)
### ——查询所有学生的sno，sname和degree列
mysql> select sno,sname,degree from course,score where course.cno = score.cno;
+-----+-----------------+--------+
| sno | sname           | degree |
+-----+-----------------+--------+
| 102 | 数字电路        |     77 |
| 105 | 数字电路        |     67 |
| 104 | 高等数学        |     87 |
| 101 | 操作系统        |     89 |
| 106 | 操作系统        |     98 |
| 103 | 计算机导论      |     87 |
| 108 | 计算机导论      |     87 |
+-----+-----------------+--------+
7 rows in set (0.01 sec)
### ——查询“95033”班级学生每门课的平均分
1.先把班级学生拿出来
2.在把1当条件到分数表里筛选
3.再以cno字段进行分组查询平均成绩
mysql> select cno,avg(degree) from score where sno in (select sno from student where class = '95033') group by cno;
+-------+-------------+
| cno   | avg(degree) |
+-------+-------------+
| 2-801 |     77.0000 |
| 2-802 |     87.0000 |
| 3-802 |     87.0000 |
+-------+-------------+
3 rows in set (0.00 sec);
### ——查询和学号为105，108的同学同年出生的所有学生的sno，sname，sbirthday。
mysql> select *from student where year(sbirthday) in (select year(sbirthday)from student where sno in(105,108));
+-----+--------------+------+---------------------+-------+
| sno | sname        | ssex | sbirthday           | class |
+-----+--------------+------+---------------------+-------+
| 101 | 撒旦法达     | 男   | 1977-04-23 00:00:00 | 95037 |
| 102 | 阿斯蒂芬     | 女   | 1977-04-23 00:00:00 | 95033 |
| 103 | 发电房芬     | 女   | 1977-04-23 00:00:00 | 95035 |
| 104 | 曾阿凡达     | 男   | 1977-04-23 00:00:00 | 95033 |
| 105 | 四方达狗     | 男   | 1977-04-23 00:00:00 | 95032 |
| 106 | 曾华         | 女   | 1977-04-23 00:00:00 | 95031 |
| 107 | 阿法狗       | 男   | 1977-04-23 00:00:00 | 95033 |
| 108 | 沙发         | 男   | 1977-04-23 00:00:00 | 95033 |
| 109 | 曾曾华       | 女   | 1977-04-23 00:00:00 | 95033 |
+-----+--------------+------+---------------------+-------+
9 rows in set (0.00 sec)
### ——查询四狗老师教课的学生的成绩
mysql> select *from score where cno=(select cno from course where tno = (select tno from teacher where tname = '四狗'));
+-----+-------+--------+
| sno | cno   | degree |
+-----+-------+--------+
| 102 | 2-801 |     77 |
| 105 | 2-801 |     67 |
+-----+-------+--------+
2 rows in set (0.00 sec)
### ——查询计算机系与电子工程系不同职称的教师的tname和prof
————union 求并集  合并到一起
select prof from teacher where depart ='电子工程系'；
select *from teacher where depart = '计算机系' and prof not in (select  prof from teacher where depart ='电子工程系')
union
select *from teacher where depart = '电子工程系' and prof not in (select  prof from teacher where depart ='计算机系');
+----+--------------+------+---------------------+-----------+-----------------+
| tno | tname        | tsex | tbirthday           | prof      | depart          |
+-----+--------------+------+---------------------+-----------+-----------------+
| 801 | 法达         | 男   | 1977-04-30 00:00:00 | 讲师      | 计算机系        |
| 803 | 房芬         | 女   | 1977-04-13 00:00:00 | 教授      | 计算机系        |
| 805 | 四狗         | 男   | 1977-04-27 00:00:00 | 副教授    | 电子工程系      |
| 806 | 曾沙发华     | 女   | 1977-05-23 00:00:00 | 助教      | 电子工程系      |
+-----+--------------+------+---------------------+-----------+-----------------+
4 rows in set (0.01 sec）
### ——any任意（至少） all且（所有） as 别名（重命名打印） union合并 having count(*)>1 数量大于1   year(now())现在的年份   order by 顺序查询
