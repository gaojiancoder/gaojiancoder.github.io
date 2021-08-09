---
title: Sevelet2
top: false
cover: false
toc: true
mathjax: true
date: 2021-08-09 17:43:31
password:
summary:
tags: notes
categories: Sevelet
---

# Sevelet

## HttpServletResponse对象

向客户端输出数据service()方法中接收的是Response接口的实例化对象，这个对象向客户端发送数据，响应头响应状态码的方法。

### 输出流的两种形式：

getWrite()获取字符流，只能响应字符

getOutputStream()获取字节流，可以响应一切数据

![1](1.png)

### 解决响应乱码问题

乱码原因：服务端进行编码采用ISO-8859-1不支持中文

指定服务端编码格式，同时指定客户端编码格式，同时设置才好使

![](2.png)

### 重定向

重定向是一种服务器指导客户端的行为。重定向中有两个请求且属于客户端行为

![](3.png)

### 请求转发和重定向区别

| 请求转发(requst.getRequestDispatcher().forward()) | 重定向(response.sendRedirect()) |
| :------------------------------------------------ | ------------------------------- |
| 一次请求，数据在request域共享                     | 两次请求，数据不共享            |
| 服务器端行为                                      | 客户端行为                      |
| 地址栏不发生变化                                  | 地址栏发生改变                  |
| 绝对地址定位到站点后                              | 绝对地址可写到http://           |

在当前资源下跳转可以重定向也可以请求转发，跨域做跳转只能重定向

共享request对象用请求转发，跳转地址不是当前项目下的只能重定向

![](4.png)

### Cookie对象

浏览器提供的技术，会存在客户端的目的是提高网络效益以及浏览器负载

#### cookie的创建

![](5.png)

#### cookie的获取

![](6.png)

#### Cookie到期时间

![](7.png)

#### Cookie注意点

1.cookie保存当前浏览器

2.cookie不支持中文，如果一定要中文需要先URLEncoder.encode()进行编码，然后通过URLDecoder.decode()解码

3.同名会覆盖

4.不同浏览器存放数量有限，也有大小限制

5.创建cookie对象都要响应response.addcookie()

#### cookie的路径

![](8.png)

总结：只有访问的路径中包含cookie对象的path值时才可以获取到

## HttpSession对象

### 总结

优点：传输数据可多次跳转，只要不销毁就可一直拿到

### 创建Session

![](9.png)

### Session域对象

![](10.png)

### Session销毁

1默认到期时间

2自己设定到期时间

3立即销毁  Session.invalidate()

4关闭浏览器就失效

5关闭服务器失效

![](11.png)

## ServletContext对象

### ServletContext对象的获取

![1](12.png)

### ServletContext域对象

![1](13.png)

## 文件的上传和下载

### 文件上传创建表单

![1](14.png)

### 后台代码实现

使用注解@MultipartConfig将一个Servlet标识为支持文件上传，Servlet将multipart/form-data的POST请求封装成Part，通过Part对上传文件的操作

![](15.png)

编写完代码后先进入文件上传的HTML中传输文件然后找到传输的指定路径

### 文件下载

#### 超链接下载

![](16.png)

后台下载

```java
@WebServlet(name = "0", value = "/0")
public class download extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("文件下载" );
        request.setCharacterEncoding("UTF-8");//设置请求的编码格式
        String fileName = request.getParameter("fileName");//获取参数(得到要下载的文件名)
        //判断参数的非空 trim()去除字符串的前后空格
        if (fileName ==null|| "".equals(fileName.trim())){
            response.setContentType("text/html;charset=UTF-8");
            response.getWriter().print("请输入要下载的文件名!");
            response.getWriter().close();
            return;
        }
        //得到要下载的资料的路径
        String Path = request.getServletContext().getRealPath("/download/");
        File file= new File(Path+fileName);//通过路径得到file对象
        //判断文件对象是否存在并且是一个标准文件
        if (file.exists() &&file.isFile()){
            response.setContentType("application/x-msdownload");//设置响应类型
            response.setHeader("Content-Dispoition","attachment;filename="+fileName);
            FileInputStream in = new FileInputStream(file);//得到file输入流
            ServletOutputStream out = response.getOutputStream();//得到字节输出流

            byte[] bytes=new byte[1024];//定义数组
            int len = 0;//定义长度
            while ((len=in.read(bytes))!=-1){
            out.write(bytes,0,len);
            }
            out.close();
            in.close();

        }else {
            response.getWriter().write("文件不存在");
            response.getWriter().close();
        }

    }
```

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"
        "http://www.w3.org/TR/html4/loose.dtd">
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>文件下载</title>
</head>
<body>
    <a href="download/14.png">文本文件</a>
    <a href="download/15.png">图片文件</a>
    <a href="download/16.png">压缩文件</a>
    <hr>
    <a href="download/14.png" download>文本文件</a>
    <a href="download/15.png" download>图片文件</a>
    <hr>
    <form action="download">
        文件名：<input type="text" name="fileName" placeholder="请输入">
        <button>下载</button>
    </form>
</body>
</html>
```

