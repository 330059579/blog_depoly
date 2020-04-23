---
title: Java自定义注解
date: 2019-12-30 15:21:54
tags: Java
description: 
cover: http://120.27.226.80:8088/images/1578473466993.jpg
top_img: http://120.27.226.80:8088/images/1578473466993.jpg
categories: Java基础
---

编写自定义直接时，需要用到java提供的一下元注解

元注解即是用来修饰注解的注解

 JDK中有一些**元注解**，主要有**@Target，@Retention,@Document,@Inherited**用来修饰注解

**@Target**

表明该注解可以应用的java元素类型

ElementType.TYPE	应用于类、接口（包括注解类型）、枚举

ElementType.FIELD	应用于属性（包括枚举中的常量）

ElementType.METHOD	应用于方法

ElementType.PARAMETER	应用于方法的形参

ElementType.CONSTRUCTOR	应用于构造函数

ElementType.LOCAL_VARIABLE	应用于局部变量

ElementType.ANNOTATION_TYPE	应用于注解类型

ElementType.PACKAGE	应用于包

ElementType.TYPE_PARAMETER	1.8版本新增，应用于类型变量）

ElementType.TYPE_USE	1.8版本新增，应用于任何使用类型的语句中（例如声明语句、泛型和强制转换语句中的类型）

**@Retention**

 表明该注解的生命周期

RetentionPolicy.SOURCE	编译时被丢弃，不包含在类文件中

RetentionPolicy.CLASS	JVM加载时被丢弃，包含在类文件中，默认值

RetentionPolicy.RUNTIME	由JVM 加载，包含在类文件中，在运行时可以被获取到

**@Document**

表明该注解标记的元素可以被Javadoc 或类似的工具文档化

**@Inherited**

表明使用了@Inherited注解的注解，所标记的类的子类也会拥有这个注解

编写测试方法如下：

```java

@Target(ElementType.FIELD)
@Retention(RetentionPolicy.RUNTIME)
public @interface ProductInfo {

    String name() default "tuanzhang";
}
```

```java
public class Order {

    @ProductInfo(name = "zhangsan")
    private String product;

    public String getProduct() {
        return product;
    }

    public void setProduct(String product) {
        this.product = product;
    }

    public static void main(String[] args) {
        Order order = new Order();
        try{
            order.setProduct("苹果");
            Field product = order.getClass().getDeclaredField("product");
            ProductInfo annotation = product.getAnnotation(ProductInfo.class);
            System.out.println(annotation.name());
        }catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

运行结果：

```
zhangsan
```

一直不懂自定义注解的应用场景，感觉自定义的注解需要用到反射获取，可以用在自己写的一些框架之中

