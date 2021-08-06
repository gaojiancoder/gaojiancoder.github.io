---
title: redis下载与安装
top: false
cover: false
toc: true
mathjax: true
date: 2021-08-06 20:34:40
password:
summary:
tags: 
categories: redis
---

# Redis安装使用流程

百度搜索install redis

![](1.png)

在终端新建文件夹执行以下命令

```java
wget http://download.redis.io/redis-stable.tar.gz
tar xvzf redis-stable.tar.gz
cd redis-stable
make
```

然后cd 到bin的前一个目录复制路径配置环境变量

![](2.png)

进入~/.zshrc中将根目录复制到最后。环境变量配置完成

百度进入mvnrepository搜索jedis下载redis依赖jar包

加入新建工程pom.xml中

![](3.png)





Lsof -i:端口号  ===查看端口运行程序

ifconfig    查看本机IP地址在en0下





