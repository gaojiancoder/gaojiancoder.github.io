---
title: mybatis第一次框架操作
top: false
cover: false
toc: true
mathjax: true
date: 2021-08-04 12:18:05
password:
summary:
tags: notes
categories: mybatis
---

# mybatis

第一章框架概况

### 1.三层架构

​	界面层：和用户打交道，接收用户的请求参数显示处理结果。（jsp，HTML，servlet）

​	业务逻辑层：接收了界面层传递的数据，计算逻辑，调用数据库，获取数据

​	数据访问层：就是访问数据库，执行对数据的查询，修改，删除等

三层对应的包：

​	界面层：controller包  （servlet）

​	业务逻辑层：service包（XXXService类）

​	数据访问层：dao包（XXXDao类）

三层中类的交互

​	用户使用界面层--->业务逻辑层---->数据访问层(持久层)---->数据库(mysql)

三层对应的处理框架

​	界面层-----servlet---->springmvc(框架)

​	业务逻辑层-----service类---->spring(框架)

​	数据访问层----dao类------>mybatis(框架)

### 2.框架

​	框架是一个舞台，是一个模板。定义好了一些功能，这些功能是可用的，可以加入项目中自己的功能，这些功能可以利用框架中写好的功能。

​	框架是针对某一领域有效的。特长是某一方面不是全能的，比如mybatis做数据库强但是不能做其他的。

mybatis框架

​	1sql mapper ：sql映射

​		可以把数据库中的一行数据映射为一个java对象

​	2。Data Access Objects(DAOs):数据访问，对数据库执行增删改查

### 3.功能

mybatis提供了哪些功能：
1提供了创建Connection,Statement,ResultSet的能力，不用开发人员自己创建了

2提供了执行SQL语句的能力

3提供了循环SQL，把SQL的结果转换为Java对象，list集合的能力

4提供了关闭资源的能力

开发人员要做的是：提供SQL语句

流程：开发人员提供SQL语句--->mybatis处理SQL----->得到list集合或java对象(表中的数据)

## 第二章入门案例

### 总体流程

![2](2.png)

## 准备工作

**1.在IDE中创建一个maven工程，补全路径下的resources包**

![1](1.png)

**2.进入mvnrepository搜索mybatis选择最新版本复制链接**

![3](3.png)

**进入ide找到pom，将上面复制的加入pom中**

![4](4.png)

**接着再在mvnrepository中搜索mysql下载mysql驱动**

![5](5.png)

**在pom中加入依赖**

![6](6.png)

**接着在pom中build下加入日志插件**

![7](7.png)

准备工作完成

## 案例实施

1创建Student

![8](8.png)

alt+回车 选择图中这两个进入里面全选

![9](9.png)

接着创建个packeg包命名为dao在里面创建StudentDao.java接口和StudentDao.xml文件

![10](10.png)

![11](11.png)

StudentDao接口中这么写

![12](12.png)

在reources中创建.xml表

![13](13.png)

![14](14.png)

![15](15.png)

![16](16.png)

创建myapp.java来运行这个命令

![17](17.png)

## 代码实现

***<u>Student</u>***

```java
package io.gaojian.bjpowernode.domain;

public class Student {
    //定义属性，要求是属性名和列名一致
    private Integer id;
    private String name;
    private String email;
    private Integer age;

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "Student{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", email='" + email + '\'' +
                ", age=" + age +
                '}';
    }
}
```

***<u>Studentdao</u>***

```java
import io.gaojian.bjpowernode.domain.Student;
import java.util.List;
public interface StudentDao {
    public List<Student> selectStudent();
}
```

***<u>StudentDao.xml</u>***

```java
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="io.gaojian.bjpowernode.dao.StudentDao">

    <select id="selectStudent" resultType="io.gaojian.bjpowernode.domain.Student">
        select id,name,email,age from student order by id;
    </select>

</mapper>
```

**<u>*Mybatis.xml*</u>**

```java
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    
    <settings>
        <setting name="logImpl" value="STDOUT_LOGGING"/>
    </settings>
    <environments default="bank">
        <environment id="bank">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/bank"/>
                <property name="username" value="root"/>
                <property name="password" value="12345678"/>
            </dataSource>
        </environment>
    </environments>

    <mappers>
        <mapper resource="io/gaojian/bjpowernode/dao/StudentsceDao.xml"/>
    </mappers>
</configuration>
```

**<u>*Pom.xml*</u>**

```java
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>io.gaojian</groupId>
    <artifactId>mybatis</artifactId>
    <version>1.0-SNAPSHOT</version>

    <name>mybatis</name>
    <!-- FIXME change it to the project's website -->
    <url>http://www.example.com</url>

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.7</version>
        </dependency>

        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.11</version>
            <scope>test</scope>
        </dependency>
        <!--mysql驱动-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.25</version>
        </dependency>


    </dependencies>

    <build>
        <resources>
            <resource>
                <directory>src/main/java</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>false</filtering>
            </resource>
        </resources>

        <pluginManagement>
            <plugins>

                <plugin>
                    <artifactId>maven-clean-plugin</artifactId>
                    <version>3.1.0</version>
                </plugin>

                <plugin>
                    <artifactId>maven-resources-plugin</artifactId>
                    <version>3.0.2</version>
                </plugin>
                <plugin>
                    <artifactId>maven-compiler-plugin</artifactId>
                    <version>3.8.0</version>
                </plugin>
                <plugin>
                    <artifactId>maven-surefire-plugin</artifactId>
                    <version>2.22.1</version>
                </plugin>
                <plugin>
                    <artifactId>maven-jar-plugin</artifactId>
                    <version>3.0.2</version>
                </plugin>
                <plugin>
                    <artifactId>maven-install-plugin</artifactId>
                    <version>2.5.2</version>
                </plugin>
                <plugin>
                    <artifactId>maven-deploy-plugin</artifactId>
                    <version>2.8.2</version>
                </plugin>
                <plugin>
                    <artifactId>maven-site-plugin</artifactId>
                    <version>3.7.1</version>
                </plugin>
                <plugin>
                    <artifactId>maven-project-info-reports-plugin</artifactId>
                    <version>3.0.0</version>
                </plugin>
            </plugins>
        </pluginManagement>
    </build>
</project>

```

**<u>*MyApp*</u>**

```java
import io.gaojian.bjpowernode.domain.Student;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.IOException;
import java.io.InputStream;
import java.util.List;

public class MyApp {
    public static void main(String[] args) throws IOException {
        String config = "mybatis.xml";
        InputStream in = Resources.getResourceAsStream(config);
        SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
        SqlSessionFactory factory = builder.build(in);
        SqlSession sqlSession =factory.openSession();
        String sqlId = "selectStudent";
        List<Student> studentList = sqlSession.selectList(sqlId);
        studentList.forEach(student -> System.out.println(student));
        sqlSession.close();

    }
}

```

