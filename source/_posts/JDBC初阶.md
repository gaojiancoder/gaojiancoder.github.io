---
title: JDBC初阶
top: false
cover: false
toc: true
mathjax: true
date: 2021-08-09 17:11:47
password:
summary:
tags: notes
categories: JDBC
---

## JDBC

### 1.概念:Java Database Connectivity    Java 数据库连接

本质:其实是官方（sun公司）定义的一套操作所有关系型数据库的规则，各个数据库的厂商去实现这套接口，提供数据库jar包。我们可以使用这套接口（JDBC）编程，真正的代码是驱动jar包中的实现类。

Eg:   person接口   worker类   person p = new worker(); 		p.eat();

### 2.快速入门：我们依然用maven进行操作

​	1.打开mvnrepository搜索mysql-connector-java

​	2.选择最新版本打开复制maven里的所有链接

![](1.png)

​	3.打开IDE创建maven工程项目

​	4.打开pom.xml文件在dependency中加入链接入下图

![](2.png)

### *代码实现

注册驱动-->连接数据库-->定义sql语句-->循环输出语句

```java
public class App {
    static final String DB_URL = "jdbc:mysql://localhost:3306/bank";
    static final String USER = "root";
    static final String PASS = "12345678";//数据库密码
    static final String QUERY = "SELECT * FROM user";

    public static void main(String[] args) {
        // Open a connection
        try (Connection conn = DriverManager.getConnection(DB_URL, USER, PASS);
             Statement stmt = conn.createStatement();
             ResultSet rs = stmt.executeQuery(QUERY);) {
            // Extract data from result set
            while (rs.next()) {
                // Retrieve by column name
                System.out.print("ID: " + rs.getInt("id"));
                System.out.print("name: " + rs.getString("name"));
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
```

到此我们已经初步的实现了JDBC操作

## 解决sql注入

```java
package jdbc;

import java.sql.*;
import java.util.HashMap;
import java.util.Map;
import java.util.Scanner;

public class JDBC {
    public static void main(String[] args) {
        Map<String, String> userLoginInfo = initUI();//创造方法管理登陆界面
        boolean loginSuccess = login(userLoginInfo); //登陆方法
        System.out.println(loginSuccess ? "登陆成功" : "登陆失败");//登陆反馈

    }

    private static boolean login(Map<String, String> userLoginInfo) {
        boolean loginSuccess = false;
        String loginName = userLoginInfo.get("loginName");
        String loginPwd = userLoginInfo.get("loginPwd");
        Connection coon = null;
        PreparedStatement ps = null;
        ResultSet rs = null;
        try {
            Class.forName("com.mysql.cj.jdbc.Driver");
            coon = DriverManager.getConnection("jdbc:mysql://localhost:3306/bank", "root", "12345678");
            String sql = "select*from login where loginName = ? and loginpwd = ?";
            ps = coon.prepareStatement(sql);


            rs = ps.executeQuery(sql);
            if (rs.next()){
                loginSuccess =true;
            }

        } catch (ClassNotFoundException | SQLException e) {
            e.printStackTrace();
        } finally {
            if (rs != null) {
                try {
                    rs.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (ps != null) {
                try {
                    ps.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (coon != null) {
                try {
                    coon.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
        return loginSuccess;
    }

    private static Map<String, String> initUI() {
        Scanner scanner = new Scanner(System.in);
        System.out.println("用户名：");
        String loginName = scanner.nextLine();
        System.out.println("密码：");
        String loginPwd = scanner.nextLine();
        //创建用户名密码
        HashMap<String, String> userLoginInfo = new HashMap<>();
        userLoginInfo.put("loginName", loginName);
        userLoginInfo.put("loginPwd", loginPwd);
        //组装map
        return userLoginInfo;
    }

}


```

