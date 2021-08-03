---
title: web工程tomcat部署
top: false
cover: false
toc: true
mathjax: true
date: 2021-08-03 23:53:17
password:
summary:
tags: notes
categories:
---

# tomcat部署

## 1.进入Tomcat官网根据电脑版本号下载tomcat

https://tomcat.apache.org/download-80.cgi

![image-1](image-1.png)

下载好后终端到一个自己指定的文件夹将其移动到其中

$ mv ~/Downloads/apache-tomcat-8.5.69.tar.gz .   

在终端中进行解压

$ tar zxvf apache-tomcat-8.5.69.tar.gz

解压完拿到文件路径后面IDE中会用到

![image-2](image-2.png)

## 2.在IDE中创建web模板

IDE中new 一个project

![image-3](image-3.png)

进入之后选中maven 勾选和选项

![image-4](image-4.png)

![image-5](image-5.png)

选完后创建标题 之后一路下一步

## 3运行Tomcat

1首先进行打包不打包无法运行 mvn clean install 

2进行配置，点击Edit![image-6](image-6.png)

点击+号 输入tom搜索 选择本地的Local

![image-7](image-7.png)

选择完local 点击FIX 出现两个选项选择第二个

![image-9](image-9.png)

之后点击OK，即可运行
