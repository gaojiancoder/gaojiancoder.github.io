---
title: MySQL学习笔记2
top: false
cover: false
toc: true
mathjax: true
date: 2021-07-26 22:51:08
password:
summary:
tags: notes
categories: MySQL
---
## mysql常用数据类型有哪些

### MySQL支持多种类型，大致可以分为三类：

#### （一）数值

类型                              字节                                   大小                                                  理解
TINYINT    	    1 byte 	             (-128，127)            (0，255)                                      小整数值    
SMALLINT          2 bytes	(-32 768，32 767)        	(0，65 535)                               大整数值    
MEDIUMINT       3 bytes	(-8 388 608，8 388 607)      (0，16 777 215)                  大整数值    
INT或INTEGER   4 bytes	(-2 147 483 648，2 147 483 647) (0，4 294 967 295)    大整数值    
BIGINT     	    8 bytes	(-9,223,372,036,854,775,808，9 223 372 036 854 775 807) 极大整数值  
FLOAT      	    4 bytes	(-3.4E+38，-1.1E-38)，0，(1.17E-38，3.4E+38)      单精度 浮点数值
DOUBLE     	    8 bytes	(-1.7E+308,-2.2E-308),0,(2.2E-308，1.7E+308)	       双精度 浮点数值

####  （二）日期                                                                                                             

日期/时间：date，time，year，datetime，timestam
类型            字节               格式                                                      理解定义
DATE            3  1000-01-01/9999-12-31     YYYY-MM-DD            日期值     
TIME            3  '-838:59:59'/'838:59:59'  HH:MM:SS             时间值或持续时间  
YEAR           1   1901/2155                YYYY                                    年份值     
DATETIME   8   1000-01-01 00:00:00\9999-12-31 23:59:59   YYYY-MM-DD HH:MM:SS 混合日期

#### （三）字符串

字符串：
  类型                           字节                                                       定义
  CHAR      	        0-255 bytes          	                               定长字符串             
  VARCHAR   	        0-65535 bytes        	                        变长字符串             
  TINYBLOB  	        0-255 bytes                          	不超过 255 个字符的二进制字符串
  TINYTEXT  	        0-255 bytes                                    	短文本字符串            
  BLOB      	        0-65 535 bytes                            二进制形式的长文本数据       
  TEXT      	        0-65 535 bytes       	                        长文本数据             
  MEDIUMBLOB	0-16 777 215 bytes   	           二进制形式的中等长度文本数据    
  MEDIUMTEXT	0-16 777 215 bytes            	            中等长度文本数据          
  LONGBLOB  	0-4 294 967 295 bytes	             二进制形式的极大文本数据      
  LONGTEXT    	0-4 294 967 295 bytes                         极大文本数据            

### 增加数据

Insert into pet values('Fluffy','Harold','n','1999-04-23',null)；
Insert into pet values('Fluf','Qing','f','1999-04-27',null)；

### 删除数据

mysql> delete from pet where name = 'Fluffy';
Query OK, 1 row affected (0.01 sec)

### 修改数据

mysql> update pet set name = '旺财' where owner = 'gao';
Query OK, 2 rows affected (0.00 sec)
Rows matched: 2  Changed: 2  Warnings: 0

mysql> select*from pet;
+---------+-------+------+------------+-------+
| name    | owner    | sex  | birth            | death |
+---------+-------+------+------------+-------+
| 旺财      | gao       | m     | 1999-03-02 | NULL  |
| Fluf       | Qing      | m     | 1999-04-27 | NULL  |
| Fffy       | Wang     | f      | 1993-04-22 | NULL  |
| Fdsdffy | Jian       | f       | 1996-09-02 | NULL  |
| 旺财      | Gao       | m    | 1945-10-02 | NULL  |
+---------+-------+------+------------+-------+
5 rows in set (0.00 sec)

### 总结一些：数据记录常见操作

——增加： insert into <表name>  values（）；
——删除：delete from <表name> where ……
——修改：update <表name> set 
——查找：select*from <表name>
