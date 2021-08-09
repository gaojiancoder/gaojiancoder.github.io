---
title: Redis初阶
top: false
cover: false
toc: true
mathjax: true
date: 2021-08-09 17:21:37
password:
summary:
tags: notes
categories: redis
---

# 																							Redis

## 												启动redis

进入redis的redis.conf目录文件下执行redis-server redis.conf

/Users/gaojian/study/redis/redis-stable

开启redis成功这时就可以输入redis-cli进入操作

## redis-cli

Redis-cli -h(输入其他电脑的IP地址可远程连接)

Redis-cli -p(端口号，开启哪个端口号默认6379)

ping操作可以查看是否连接成功PONG

Select (number)切换数据库

![](1.png)

### string类型操作

存储set(一个)mset(多个)

获取get(一个)mget(多个)

![](2.png)

### hash类型操作

#### 存取

![](3.png)

```redis
hgetall user
 "name"
 "lisi"
 "age"
 "18"
 "sex"
 "1"
```

删除hdel user name age

### list类型

分为左添加(lpush)和右添加(rpush)

![](4.png)

获取值：lrange

查询几条语句：llen

删除：lrem （数量及格） （什么内容） 从左往右开始删除

### set类型

乱序 

添加数据saad letters (aaa nnn ccc jjj)

查询数据smembers letters

查询数量 scard letters

删除 srem letters ()

### Sorted set 数据类型

根据分数来排序  集合类型

![](5.png)

查询数量 zcard score

删除 zrem score 

查询数据zrange

### 通用命令

#### 层级关系目录形式存储数据

get cart：user01：item01 apple

#### 失效时间

Redis有四个不同的命令可以用于设置键的生存时间（存在多久）或过期时间（什么时候删除）

EXPIRE<key><ttl>:用于将键key的生存时间设置为ttl秒

PEXPIRE<key><ttl>:用于将键key的生存时间设置为ttl毫秒

EXPIREAT<key><timestamp>:用于将key的过期时间设置为timestamp所指定的秒数时间戳

PEXPIREAT<key><timestamp>:用于将key的过期时间设置为timestamp所指定的毫秒数时间戳

ttl获取的值为-1说明此key没有设置有效期，永不失效。当值为-2时证明过了有效期

![](6.png)

Expire 为没有设置时间的值设置时间

#### 删除：del

## IDE创建redis配置

### 配置pom.xml

![](7.png)

#### 配置.application.properties文件

![](8.png)

#### 连接jedis

![](9.png)

#### jedis连接pool池

![](10.png)

### 封装jedisutil对外提供连接对象获取的方法

先创建test.java

![](11.png)

```java
import org.junit.After;
import org.junit.Before;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.junit4.SpringRunner;
import redis.clients.jedis.Jedis;
import redis.clients.jedis.JedisPool;
import javax.swing.*;
@SpringBootTest
@RunWith(SpringRunner.class)
public class TestJedis {
    @Autowired
    public JedisPool jedisPool;
    private Jedis jedis = null;
    //初始化jedis实例对象
    @Before
    public void initConnt() {
        jedis = jedisPool.getResource();
    }
   @After
    public void closeConnt() {
        if (null != jedis) {
            jedis.close();
        }
    }
}
```

在创建配置文件config配置文件

![](13.png)



```java
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import redis.clients.jedis.JedisPool;

@Configuration
public class RedisConfig {
    @Value("${redis.host}")
    private String host;
    @Value("${redis.port}")
    private int port;

    @Bean
    public JedisPool jedisPool() {
        return new JedisPool(host, port);
    }
}
```



最后需要在.java文件中加入@SpringBootApplication才是封装pool池成功

![](14.png)

### jdesi操作

#### 操作string类型

```java
 @Test
    public void test(){
        jedis.set("name","zhang");
        String name = jedis.get("name");
        System.out.println(name);
   //参数的奇数是key偶数是value
        jedis.mset("address","sh","sex","1");
        List<String> list = jedis.mget("address", "sex");
        list.forEach(System.out::println);
				//通用删除
        jedis.del("name");
    }
```

#### 操作hash类型

```java
 @Test
    public void hash(){
        jedis.hset("user","name","gaojian");
        String hget = jedis.hget("user", "name");
        System.out.println(hget);
        Map<String ,String> map =new HashMap<>();
        map.put("sex","1");
        map.put("age","20");
        jedis.hmset("user",map);
        List<String> hmget = jedis.hmget("user", "age", "sex");
        hmget.forEach(System.out::println);

        Map<String, String> user = jedis.hgetAll("user");
        user.entrySet().forEach(e->{
            System.out.println(e.getKey()+"--->"+e.getValue());
        });
        jedis.hdel("user","name");
    }
```

#### 操作list

```java
  @Test
    public void testList(){
        jedis.lpush("studunt","zhang","高");
        jedis.rpush("student","hao","ni");
        List<String> student = jedis.lrange("student", 0, 3);
        student.forEach(System.out::println);
        Long llen = jedis.llen("student");
        System.out.println(llen);
        jedis.lpop("student");//左弹出
        jedis.rpop("student");//右弹出
            }
```

#### 操作set

```java
 @Test
    public void testSet(){
      jedis.sadd("letters","11","22");
        Set<String> set = jedis.smembers("letters");
        set.forEach(System.out::println);
        Long scard = jedis.scard("letters");//查数量
        System.out.println(scard);
        jedis.srem("letters","11");//删除
    }
```

#### 操作Sorted set

```
    @Test
    public void sorted(){
        Map<String ,Double> map = new HashMap<>();
        map.put("zhang",7D);
        map.put("li",1D);
        map.put("zh",3D);
        map.put("ang",5D);
        map.put("san",9D);
        jedis.zadd("asd",map);
        Set<String> set = jedis.zrange("asd", 0, 4);
        set.forEach(System.out::println);
        jedis.zrem("asd","san");//删除
```

#### 失效时间

```java
@Test
    public void testEx(){
        jedis.expire("cord",30);//给已经存在的key设置失效时间
        //查看失效时间
        jedis.ttl("cord");
        //xx,nx用法
        SetParams setParams =new SetParams();
        setParams.nx();//不存在才能成功
        setParams.xx();//存在才可以成功
        setParams.ex(30);
        jedis.set("code","test",setParams);
    }
```

#### 层级目录

```java
 @Test
    public void testDir(){
        jedis.set("card:user01:item01","apple");
        System.out.println(jedis.get("card:user01:item01"));//层级目录
    }
```

#### 获取所有key+事务

```java
 @Test
    public void testAllkey(){
        Long size = jedis.dbSize();//当前数据库key的数量
        System.out.println(size);
        Set<String> keys = jedis.keys("*");//查询当前数据库的所有key
        keys.forEach(System.out::println);
    }
```

```java
    @Test
    public void testMulti(){
        Transaction tx =jedis.multi();//开启事务
        tx.set("tel","10086");
        tx.exec();//提交事务
//        tx.discard();//回滚事务
    }
```

#### redis的持久化

bgsave 优点：可以进行保存，突然崩溃数据也不会消失。缺点：需要频繁的使用

aof：优点：时时保存数据。缺点：随时间积累数据越来越大 从新启动会非常慢

