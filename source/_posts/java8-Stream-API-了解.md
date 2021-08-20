---
title: java8 Stream API 了解
top: false
cover: false
toc: true
mathjax: true
date: 2021-08-16 22:52:20
password:
summary:
tags: notes
categories: Stream
---

## Lambda&&Stream

### 优点

简化我们匿名内部类的调用

简化之前

```java
new Thread(new Runnable() {
            @Override
            public void run() {
                System.out.println(Thread.currentThread().getName()+"你好");
            }
        }).start();
    }
```

简化之后

```java
new Thread(() -> System.out.println(Thread.currentThread().getName() + "你")).start();
```

### 依赖于函数接口

1.在接口中只允许有一个抽象的方法

2.@FunctionalInterface标记为该接口为函数接口

3.可以通过default修饰为普通方法

### 方法调用

()   参数列表

->   分隔

{}方法体

无参调用

```java
public class StreamDemo1 {
    public static void main(String[] args) {
    new Thread(()->System.out.println(Thread.currentThread().getName()+"nih"))
      .start();
		}
}
```

有参调用代码实现

```java
public class StreamDemo1 {
    public static void main(String[] args) {
        youcan youcan1=(i,j)-> {return i+"---"+j;};
        System.out.println(youcan1.get(3, 5));
    }

@FunctionalInterface
private interface youcan{
        String get(int i,int j);
		}
}
```

精简

```java
public class StreamDemo1 {
    public static void main(String[] args) {
       String s=((youcan)(i,j)-> i+"---"+j).get(2,3);
      System.out.println(s);
    }

@FunctionalInterface
private interface youcan{
        String get(int i,int j);
		}
}
```

代码案例ArrayList

forEach遍历数组

```java
public class ArrayLIst {
    public static void main(String[] args) {
        ArrayList<String> list = new ArrayList<String >();
        list.add("d");
        list.add("a");
        list.add("s");
        list.forEach(s -> {
            System.out.println(s);
        });
    }
}
```

## Stream

Stream：非常方便精简的形式遍历集合实现过滤，排序等

### API

#### 1.filter:通过设置条件过滤元素

Conrains():找出包含()中的元素

collect：终止操作符，把流元素集合起来

```java
public void filter() {
        List<String> string = Arrays.asList("asd", "asdf", "213");
        List<String> s = string.stream().filter(str -> str.contains("s")).collect(Collectors.toList());
        System.out.println(s);}
//[asd, asdf]
```

#### 2.distinct:去重,比较的是两个数的地址

```java
public void distinct() {
    List<String> string = Arrays.asList("asd", "asdf", "213", "213", "123123");
    List<String> collect = string.stream().distinct().collect(Collectors.toList());
    System.out.println(collect);
}
//[asd, asdf, 213, 123123]
```

#### 3.limit：返回一个不超过给定长度的流

```java
public void limit() {
    List<String> string = Arrays.asList("asd", "asdf", "213", "213", "123123");
    List<String> collect = string.stream().limit(2).collect(Collectors.toList());
    System.out.println(collect);
}
//[asd, asdf]
```

#### 4.skip:丢掉前几个元素

```java
public void skip() {
List<String> string = Arrays.asList("asd", "asdf", "213", "213", "123123");
List<String> collect = string.stream().skip(2).collect(Collectors.toList());
System.out.println(collect);
}
//[213, 213, 123123]
```

#### 5.map:对流中所有元素做统一处理

接收函数为参数，函数会被应用到每个元素上

Concat:在元素后加后缀

```java
public void map() {
    List<String> string = Arrays.asList("asd", "asdf", "213", "213", "123123");
    List<String> list = string.stream().map(str -> str.concat("__heima")).collect(Collectors.toList());
    System.out.println(list);
}
//[asd__heima, asdf__heima, 213__heima, 213__heima, 123123__heima]
```

#### 6.flatMap：扁平化一个流，把各个数组的流合并起来成一个流

#### 7.sorted：返回排序后的流

排序需要的第三方jar包

```xml
 <dependency>
      <groupId>commons-collections</groupId>
      <artifactId>commons-collections</artifactId>
      <version>3.2.2</version>
    </dependency>
```

```java
 public void sorted(){
        List<String> string = Arrays.asList("asd", "asdf", "213", "213", "123123");
        List<String> collect = string.stream().sorted().collect(Collectors.toList());
        System.out.println(collect );//[123123, 213, 213, asd, asdf]
        System.out.println("----------------");
        List<String> int1 = Arrays.asList("40", "-312", "-32", "213", "123");
        List<String> collect1 = int1.stream().sorted().collect(Collectors.toList());
        System.out.println(collect1);//[-312, -32, 123, 213, 40]
        System.out.println("----------");
        List<String> int2 = Arrays.asList("答复", "阿道夫", "奥", "威风", "好的");
        List<String> collect2 = int2.stream().sorted(Collator.getInstance(Locale.CHINA)).collect(Collectors.toList());
        System.out.println(collect2);
   			//[阿道夫, 奥, 答复, 好的, 威风]
}
```

### 终止操作符

#### 1.anyMatch:检查是否至少匹配一个元素

```java
public void anymatch(){
    List<String> string = Arrays.asList("asd", "asdf", "213", "213", "123123");
    boolean bc = string.stream().anyMatch(s -> s.contains("2"));
    System.out.println(bc);
    }//ture
```

#### 2.allMatch：是否匹配所有元素

#### 3.noneMatch:没有匹配 所有元素都不满足条件

#### 4.findAny：返回当前流的任意元素

```java
public void findAny(){
    List<String> string = Arrays.asList("asd", "asdf", "213", "213", "123123");
        Optional<String> any = string.stream().findAny();
        System.out.println(any.get());
}
```

#### 5.forEach：遍历

```java
public void forEach() {
    List<String> string = Arrays.asList("asd", "asdf", "213", "213", "123123");
    string.stream().forEach(s -> System.out.println(s));
}
/**
 * asd
 * asdf
 * 213
 * 213
 * 123123
 */
```

#### 6.collect:将流转换成其他模式

#### 7.reduce：将流中的元素反复结合起来，得到一个值

```java
public void reduce() {
    List<String> string = Arrays.asList("asd", "asdf", "213", "213", "123123");
    Optional<String> reduce = string.stream().reduce((end, item) -> {return end + item;});
    System.out.println(reduce);
}
//Optional[asdasdf213213123123]
```

#### 8.count:获取集合中元素数量

