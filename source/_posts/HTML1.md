---
title: HTML1
top: false
cover: false
toc: true
mathjax: true
date: 2022-11-21 19:13:34
password:
summary:
tags: Study
categories: javaWeb
---

# Html

## 1.适合编写html的便利软件

HBuilderx：https://www.dcloud.io/hbuilderx.html

## 2.什么是html

- 超文本标记语言
- 并不是编程语言

### 2.1网络三要素：协议，IP，端口

- http://www.baidu.com:80（对标）



### 2.2html的每一个标签都是成对出现

```html
<!-- html标签是html网页的根标签 -->
<html> 
	<head>
		<title>第一个html</title>
	</head>
	
	<!-- 我们主要关注这个body标签 -->
	<body>
		网页中的内容
	</body>
</html>
```

## 3.标签介绍

### 3.1 标签含义

```txt
<!DOCTYPE html> : 表示本文档遵循html5规范
<html> ： 表示一个网页根标签/根元素（root element）
<head> : 定义了一个html网页的头信息，主要是一些元数据【描述或者管理数据的数据】
<meta charset="utf-8"> : 设置浏览器以哪种编码来解析网页，如果不设置就有可能会产生乱码
<title> : 设置浏览器的标签页的显示
<body> : HTML所有要显示在网页的内容
```

### 3.2 单标签和双标签

```html
双标签：<title>双标签内部</title>
单标签：<hr>，<br>
  一般不需要在双标签内部输入内容时都可以简写为单标签
```

## 4.需要了解的见标签

### 4.1标题标签，段落标签，换行标签以及水平线标签

```html
<h1>一级标题</h1>
<h2>二级标题</h2>
...一直到6级标题


<p>这里添加的内容为一个段落 </p>
<p>可以有多个p标签</p>
相当于换行的功能

换行标签  在需要换行的位置添加<br>就可以自动换行
<br>

水平线标签添加一条线	单标签 <hr/>中的修饰内容可加可不加 
<hr size="5" color="red" aign="left"/>
```

### 4.2字体标签，特殊字符，文本格式标签以及锚点

```html
字体标签
	<font size ="5" ;color="red"; face="宋体">这里是要改变的字体</font>

特殊字符
	&lt &gt
	<     >  转义字符 &lt就是<  &gt就是>的意思
    
文本格式标签
	粗体<strong>粗体</strong>
    斜体<em>斜体</em>
    删除线<del>删除线</del>
    下划线<u>下划线</u>
    大号<big>大号字体</big>
    小号<small>小号字体</small>
    
        
锚点
    <a name="top"></a>
    <a harf="#top">回到top这个定位了锚点的位置</a> （相当于回到顶部）
```

## 5.重要的标签

### 5.1 div 和span

- div和span都属于容器标签 可以用来存放任何东西 包括标签
- 区别
  - div换行
  - span不换行  多个span共享一行

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title>标题</title>
	</head>
	<body>
		<div style="border: 1px solid red; width: 300px; height: 200px;">
			这里是div的内容在这里添加一个span容器容器的内容是我的span
			<span style="border: 1px solid blue;">我是span</span>
		</div>
		<div>
			这里是第二个div
		</div>
		<span style="border: 1px solid red;">这里是div外的span容器</span>	
	</body>
</html>
```

### 5.2 图片和超链接以及列表标签

- 图片标签

```html
<body>
    <img src="这里放图片的路径" width="200" heigth="200" alt="当图片不能显示时出现的提示信息" title="这里是鼠标放在图片上的提示信息">
</body>
```

- 超链接标签

```html
<body>
    <a hraf="这里放需要跳转的链接路径 可以是绝对路径也可以是相对路径" target="_self">超链接</a>
</body>
target : 开启新链接的方式
	_blank: 在浏览器中打开一个新的窗口
	_parent: 在父窗口打开
	_self: 在当前窗口打开，默认
	_top: 在整个浏览器中打开链接，删除其他所有框架
```



- 列表标签
  - 列表分为有序列表和无序列表

```html
1.有序列表
	有序可选项有: 数字 大小写字母 大小写阿拉伯数组i或者I
<body>
    <ol type = "1">
       <li>新闻</li> 
        <li>娱乐</li>
    </ol>
</body>
2.无序列表
	无序列表可选项：
		空心圆：circle
		实心圆：disc
		黑色实心方块：square
<body>
    <ul type="disc">
        <li>体育</li>
    </ul>
</body>
```



### 5.3表格标签

#### 1.表格的结构化标签

1. caption：表格的标题
2. thead：表格的头部
3. tbody：表格的整体
4. tfoot：表格的尾部

#### 2.表格的标签

1. table：表格的根标签
2. tr：   一个tr标签就表示一行
3. td：   一个td标签表示一个单元格 
   1. th和td一样 不过th用于thead中用来表示标题格

#### 3.标签的属性

1. border ：边框值 可选”0“或者“1”
2. borderColor ：边框颜色
3. align ：   表格对齐功能   center    left    right
4. cellpadding :  单元格填充，单元格与内容之间的间距
5. cellspacing ： 单元格和单元格之间的距离

#### 4.跨行和跨列

1. colspan：横向合并
2. rowspan：纵向合并

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>标题</title>
	</head>
	<body>
		<table border="1" cellspacing="0" cellpadding="10" align="center">
			<!-- 标题标签 -->
			<caption>每日工作计划表</caption>
			<!-- 表格头部 -->
			<thead>
				<tr>
					<th>序号</th>
					<th>工作事项</th>
				</tr>
			</thead>
			<!-- 表格主体 -->
			<tbody>
				<tr align="center">
					<td>1</td>
					<td></td>
				</tr>
			</tbody>
			<!-- 表格底部 -->
			<tfoot>
				<tr align="center">
					<td colspan="2">版权所有</td>
				</tr>
			</tfoot>
		</table>
	</body>
</html>
```



### 5.4表单标签

form元素就是html中的表单标签。

```html
<form action="提交的表单的目标的地址" method="post">
    ....
</form>
```

5.4.1 表单标签中常用的标签框

- 文本框
- 密码框
- 单选框
- 复选框
- 普通按钮
- 提交按钮
- 重置按钮
- 上传选项
- 下拉列表
- 文本域
- 隐藏域

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>表单标签的标签框练习</title>
	</head>
	<body>
		<form>
			<!-- 文本框 -->
			<p>
				<!-- placeholder ： 提示文字 -->
				用户名称: <input type="text" name="username" placeholder="请输入用户名称"/>
			</p>
			<!-- 密码框 -->
			<p>
				<!-- placeholder ： 提示文字 -->
				用户密码: <input type="password" name="password" placeholder="请输入用户密码"/>
			</p>
			<!-- 单选框 -->
			<p>
				<!-- 
				name ： 单选框的名字必须要相同，否则就无法实现单选 
				value : 手动的给单选项赋值
				checked="checked" : 加上了这句话表示默认的选项
				-->
				性别: <input type="radio" name="gender" value="1" checked="checked"/>男
					<input type="radio" name="gender" value="2"/>女
			</p>
			<!-- 复选框 -->
			<p>
				爱好:<input type="checkbox" name="hobby" value="basketball"/>篮球
					<input type="checkbox" name="hobby" value="football"/>足球
					<input type="checkbox" name="hobby" value="pingpang"/>乒乓球
			</p>
			
			<!-- 上传选项 -->
			<p>
				文件上传:<input type="file" name="user"/>
			</p>
			
			<!-- 下拉列表 
			multiple : 开启下拉列表的多选模式
			-->
			<select name="addr" >
				<option value="china">中国</option>
				<option value="japan">小日子</option>
				<option value="korea">棒子</option>
			</select>
			
			<!-- 文本域 -->
			<textarea name="info" rows="10" cols="10"></textarea>
			
			<!-- 隐藏域 -->
			<input type="hidden" value="123" name="hid"/>
			
			<!-- 按钮 -->
			<input type="button" value="我很普通"/>
			<input type="reset" value="重置"/>
			
			<!-- 提交表单 -->
			<input type="image" src="img/001.png"/>
			<input type="submit" value="提交"/>
		</form>
	</body>
</html>
```

