---
title: maven创建与使用
top: false
cover: false
toc: true
mathjax: true
date: 2021-07-29 10:21:26
password:
summary:
tags: notes
categories:
---
## 1下载maven
输入以下网址进入maven官网 
```
maven.apache.org
```
![](2.png)
点击进入Download下滑找到第一个是Linux第二个是Windows版本
![](3.png)
下载完解压即可
## 2配置path和环境路径
在终端输入mvn确定mvn是否配置了环境变量
![](1.png)
这是已经配置完成可以直接使用mvn。
如果没有出现该界面则到下载解压文件所在位置将路径加入到path中即可
输入该命令下载maven运行环境.m2路径
```
mvn help:system
```
![](4.png)
运行完毕   将下载解压下的文件settings.xml移动到.m2中
## 3创建maven IDE运行模板
创建一个新文件夹在当前文件夹中输入
```
mvn archetype:generate -DgroupId=io.gaojian -DartifactId=demo -DarchetypeArtifactId=maven-archetype-quickstart -DarchetypeVersion=1.4 -DinteractiveMode=false
```
输入此配置为不需要交互一切按默认设置算
![](5.png)
```
mvn archetype:generate -DarchetypeArtifactId=maven-archetype-quickstart -DarchetypeVersion=1.4  
```
此配置为交互模式，模板名称，项目名称，版本号可以自己设置
![](6.png)
模板名称要倒写，创建完毕
## 4.IDE运行
进入IDE 找到open 打开
![](7.png)
找到创建模板的文件夹位置点击open
![](8.png)
即操作完成
## 5.maven操作
可以在软件最右边找到maven按钮 会出现快捷操作按钮
![](9.png)
双击即可运行
也可以通过terminal来输入mvn语句执行
![](10.png)
## 6.打包与运行
当运用maven写完一个程序或者项目时需要将成品发送给需要的人 这时就需要打包
![](11.png)
maven中install提供了打包的程序双击执行这时文件夹下会出现target文件夹 这就是已经打包完毕可以传输的了
![](12.png)
在当前文件下可用java -cp+项目名-版本号.jar +模板名.app来执行
![](13.png)
