---
title: Java的内部类
date: 2019-12-17 11:04:06
tags: Java
description: 
cover: http://120.27.226.80:8088/images/1578473569124.jpg
top_img: http://120.27.226.80:8088/images/1578473569124.jpg
categories: Java基础
---

**重点**

内部类可以分为四类：普通内部类、静态内部类、匿名内部类、局部内部类

普通内部类不能创建，必须先创建外部类

静态内部类也是作为一个外部类的静态成员而存在，创建一个类的静态内部类对象不需要依赖其外部类对象

普通内部类不能定义静态成员变量

普通内部类和外部类的成员变量，不管是何种修饰符修饰，均可互相调用

静态内部类可以定义静态与费静态的变量

静态内部类的变量在任何地方均可以访问（静态成员变量直接 类名.变量名，非静态成员变量先实例化内部类）

静态内部类只能访问外部类的静态成员变量

匿名内部类中可以使用外部类的属性，但是外部类却不能使用匿名内部类中定义的属性，因为是匿名内部类，因此在外部类中无法获取这个类的类名，也就无法得到属性信息。

**普通内部类**

这个是最常见的内部类之一了，其定义也很简单，在一个类里面作为类的一个字段直接定义就可以了，例：

```java
public class TestClass {

    public TestClass() {
        System.out.println("外部类TestClass创建");
    }

    public class TestInnerClass {
        public TestInnerClass() {
            System.out.println("创建TestInnerClass对象");
        }
    }
}
```

普通内部类不能创建，必须先创建外部类

```java
 public static void main(String[] args) {
        TestClass testClass = new TestClass();
        TestInnerClass testInnerClass = testClass.new TestInnerClass();
    }
```

普通内部类与外部类的变量调用

```java
public class TestClass {

    public int outField1 = 1;
    protected int outField2 = 2;
    int outField3 = 3;
    private int outField4 = 4;


    public TestClass() {
        TestInnerClass innerObj = new TestInnerClass();
        System.out.println("创建 " + this.getClass().getSimpleName() + " 对象");
        System.out.println("其内部类的 field1 字段的值为: " + innerObj.field1);
        System.out.println("其内部类的 field2 字段的值为: " + innerObj.field2);
        System.out.println("其内部类的 field3 字段的值为: " + innerObj.field3);
        System.out.println("其内部类的 field4 字段的值为: " + innerObj.field4);
        System.out.println("外部类TestClass创建");
    }

    public class TestInnerClass {
        public int field1 = 5;
        protected int field2 = 6;
        int field3 = 7;
        private int field4 = 8;
        public TestInnerClass() {
            System.out.println("创建 " + this.getClass().getSimpleName() + " 对象");
            System.out.println("其外部类的 outField1 字段的值为: " + outField1);
            System.out.println("其外部类的 outField2 字段的值为: " + outField2);
            System.out.println("其外部类的 outField3 字段的值为: " + outField3);
            System.out.println("其外部类的 outField4 字段的值为: " + outField4);
        }
    }


    public static void main(String[] args) {
        TestClass testClass = new TestClass();
    }
}
```

普通内部类和外部类的成员变量，不管是何种修饰符修饰，均可互相调用

**静态内部类**

一个类的静态成员独立于这个类的任何一个对象存在，只要在具有访问权限的地方，我们就可以通过 类名.静态成员名 的形式来访问这个静态成员，同样的，静态内部类也是作为一个外部类的静态成员而存在，创建一个类的静态内部类对象不需要依赖其外部类对象。例：

```java
public class TestClass2 {

    public static  int field1 = 1;

    public TestClass2() {
        System.out.println("外部类创建");
    }

    static class TestStaticClass {
        public TestStaticClass() {
            System.out.println("内部类创建");
        }
    }
    
    public static void main(String[] args) {
        TestStaticClass staticClass = new TestStaticClass();
    }
}

```

**静态内部类的成员变量访问**

```java
public class TestClass2 {

    public static  int field1 = 1;

    public TestClass2() {
        System.out.println("外部类创建");
        TestStaticClass innerObj = new TestStaticClass();
        System.out.println("其内部类的 field2 字段的值为: " + innerObj.field2);
        System.out.println("其内部类的 field3 字段的值为: " + innerObj.field3);
        System.out.println("其内部类的 field4 字段的值为: " + innerObj.field4);
    }

    static class TestStaticClass {
        protected int field2 = 2;
        int field3 = 3;
        private int field4 = 4;
        // 静态内部类中可以定义 static 属性
        static int field5 = 5;

        public TestStaticClass() {
            System.out.println("内部类创建");
            //静态内部类只能引用外部类的静态成员变量，而不能引用非静态成员变量
            System.out.println("其外部类的 field1 字段的值为: " + field1);
        }
    }

    public static void main(String[] args) {
        System.out.println(new TestStaticClass().field4);
        System.out.println(TestStaticClass.field5);
    }
}

```

可以看到，静态内部类就像外部类的一个静态成员一样，创建其对象无需依赖外部类对象（访问一个类的静态成员也无需依赖这个类的对象，因为它是独立于所属类的对象的）。但是于此同时，静态内部类中也无法访问外部类的非静态成员，因为外部类的非静态成员是属于每一个外部类对象的，而本身静态内部类就是独立外部类对象存在的，所以静态内部类不能访问外部类的非静态成员，而外部类依然可以访问静态内部类对象的所有访问权限的成员，这一点和普通内部类无异。

**匿名内部类**

匿名内部类有多种形式，其中最常见的一种形式莫过于在方法参数中新建一个接口对象 / 类对象，并且实现这个接口声明 / 类中原有的方法了：

首先定义一个类如下

```java
public class TestClass3 {

    public void test(){
        System.out.println("外部类public方法");
    }

    public final void test2(){
        System.out.println("外部类final方法");
    }

    public void test3(){
        System.out.println("外部类private方法");
    }
}
```

编写匿名内部类

```java
public class NameLessClass {

    public void loadClass(){
        TestClass3 class3 = new TestClass3() {

            public void test() {
                System.out.println("重写外部类public方法");
            }

            public void test3() {
                System.out.println("重写外部类private方法");
            }
        };

        class3.test();
        class3.test2();
        class3.test3();

    }


    public static void main(String[] args) {
        NameLessClass nameLessClass = new NameLessClass();
        nameLessClass.loadClass();
    }
}

```

```
重写外部类public方法
//没有重写仍然执行外部类方法
外部类final方法
重写外部类private方法
```

重写接口的内部类写法同上

上面的代码中展示了常见的两种使用匿名内部类的情况：

1、直接 new 一个接口，并实现这个接口声明的方法，在这个过程其实会创建一个匿名内部类实现这个接口，并重写接口声明的方法，然后再创建一个这个匿名内部类的对象并赋值给前面的 OnClickListener 类型的引用；

2、new 一个已经存在的类 / 抽象类，并且选择性的实现这个类中的一个或者多个非 final 的方法，这个过程会创建一个匿名内部类对象继承对应的类 / 抽象类，并且重写对应的方法。

同样的，在匿名内部类中可以使用外部类的属性，但是外部类却不能使用匿名内部类中定义的属性，因为是匿名内部类，因此在外部类中无法获取这个类的类名，也就无法得到属性信息。

**局部内部类**

局部内部类使用的比较少，其声明在一个方法体 / 一段代码块的内部，而且不在定义类的定义域之内便无法使用，其提供的功能使用匿名内部类都可以实现，而本身匿名内部类可以写得比它更简洁，因此局部内部类用的比较少。

**内部类的嵌套**

内部类的嵌套，即为内部类中再定义内部类，这个问题从内部类的分类角度去考虑比较合适：

普通内部类：在这里我们可以把它看成一个外部类的普通成员方法，在其内部可以定义普通内部类（嵌套的普通内部类），但是无法定义 static 修饰的内部类，就像你无法在成员方法中定义 static 类型的变量一样，当然也可以定义匿名内部类和局部内部类；

静态内部类：因为这个类独立于外部类对象而存在，我们完全可以将其拿出来，去掉修饰它的 static 关键字，他就是一个完整的类，因此在静态内部类内部可以定义普通内部类，也可以定义静态内部类，同时也可以定义 static 成员；

匿名内部类：和普通内部类一样，定义的普通内部类只能在这个匿名内部类中使用，定义的局部内部类只能在对应定义域内使用；

局部内部类：和匿名内部类一样，但是嵌套定义的内部类只能在对应定义域内使用。





**内部类和多重继承**

我们已经知道，Java 中的类不允许多重继承，也就是说 Java 中的类只能有一个直接父类，而 Java 本身提供了内部类的机制，这是否可以在一定程度上弥补 Java 不允许多重继承的缺陷呢？我们这样来思考这个问题：假设我们有三个基类分别为 A、B、C，我们希望有一个类 D 达成这样的功能：通过这个 D 类的对象，可以同时产生 A 、B 、C 类的对象，通过刚刚的内部类的介绍，我们也应该想到了怎么完成这个需求了，创建一个类 D.java：

```java
class A {
    public void testA(){
        System.out.println("A类方法");
    }
}

class B {
    public void testB(){
        System.out.println("B类方法");
    }
}

class C {
    public void testC(){
        System.out.println("C类方法");
    }
}
```

```java
public class D extends A {

    // 内部类，继承 B 类
    class InnerClassB extends B {
    }

    // 内部类，继承 C 类
    class InnerClassC extends C {
    }

    // 生成一个 B 类对象
    public void makeB() {
        new InnerClassB().testB();
    }

    // 生成一个 C 类对象
    public void makeC() {
        new InnerClassC().testC();
    }


    public static void main(String[] args) {
        D d = new D();
        d.makeB();
        d.makeC();
    }
}
```

输出结果：

```
B类方法
C类方法
```

因此这种方式在某种程度上可以说是 Java 多重继承的一种实现机制。但是这种方法也是有一定代价的，首先这种结构在一定程度上破坏了类结构，一般来说，建议一个 .java 文件只包含一个类，除非两个类之间有非常明确的依赖关系（比如说某种汽车和其专用型号的轮子），或者说一个类本来就是为了辅助另一个类而存在的（比如说上篇文章介绍的 HashMap 类和其内部用于遍历其元素的 HashIterator 类），那么这个时候使用内部类会有较好代码结构和实现效果。而在其他情况，将类分开写会有较好的代码可读性和代码维护性。