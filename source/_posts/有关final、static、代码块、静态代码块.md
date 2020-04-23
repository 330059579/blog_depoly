---

title: 有关final、static、代码块、静态代码块
date: 2019-12-30 14:48:48
tags: Java
description: 
cover: https://pic1.zhimg.com/80/v2-a0620b47fcea13878cce382659529959_hd.jpg
top_img: https://pic1.zhimg.com/80/v2-a0620b47fcea13878cce382659529959_hd.jpg
categories: Java基础
---

**final**

- **作用在类上**   则为final类，final类不能被继承。一般用于工具类时，同时把工具类构造函数声明为私有，暴露静态共有方法
- **作用在成员变量上**    则视为常量。此时赋值方式有三种：（1）声明时赋值（2）构造函数中赋值（3）代码块中赋值。 即不管哪种方式都要保证在使用该变量之前要确保已经有值。使用该特性，可以强制赋值。final变量因为不可变，所以可以安全的存在于多线程中。
- **作用在方法上**           作用在方法上可以保证该方法不能被重写
- **作用在参数上**           保证在方法体内部参数值不会被再次赋值，一般好的编程习惯应该把参数值视为final，不管有没有显示使用final(重构)

**static**

- **作用在类上**  修饰类则为静态类，但只能修饰内部类，修饰外部类时不能通过编译
- **作用在方法上**， 修饰方法则为静态方法，静态方法不能引用非静态变量，因为不能确定静态变量有没有初始化。
- **作用在变量上**  为静态变量，可以用类名.变量名来修饰， 静态变量一般使用final来修饰，因为静态变量隶属于该类的所有对象，如果可以随意更改，会存在较大风险

 **举个栗子**

```java
public class TestClass4 {

    public static boolean flag = true;

    public static void main(String[] args) {
        TestClass4 a = new TestClass4();
        TestClass4 b = new TestClass4();
        if (a.flag) {
            System.out.println("a_flag=" + a.flag);
            a.flag = false;
        }

        System.out.println("b_flag=" + b.flag);
    }
}
```

运行结果：

```
a_flag=true
b_flag=true
```

当静态变量不适用final修饰时，一个对象改变了flag的值，其他对象也会跟着改变

如果使用final修饰:

```java
 public final boolean flag = true;
```

修改时编译不通过

**代码块**

代码块分为普通代码块和构造代码块

普通代码块：在方法或语句中出现的{}，用的比较少。执行顺序是按声明顺序执行。

```java
 public static void main(String[] args) {
        {
            System.out.println("普通代码块-先声明");
        }
        System.out.println("函数普通");
        {
            System.out.println("普通代码块-后声明");
        }
    }
```

构造代码块：直接在类中定义且没有static关键字的代码块，比较常用。执行顺序是在构造函数执行之前执行，每声明一个对象都会执行一次。

```java
public class CodeBlock {
    {
        System.out.println("构造代码块-先声明-执行");
    }
    public CodeBlock(){
        System.out.println("构造器执行");
    }
    {
        System.out.println("构造代码块-后声明-执行");
    }
    public void plainFunc(){
        System.out.println("普通方法执行");
    }
    public static void main(String[] args) {
        System.out.println("main方法");
        CodeBlock cb1 = new CodeBlock();
        cb1.plainFunc();
        CodeBlock cb2 = new CodeBlock();
    }
}
```

```java
main方法
//不管构造代码块在什么位置，倒在构造方法之前执行
构造代码块-先声明-执行
构造代码块-后声明-执行
构造器执行
普通方法执行
//每实例化一个对象就会执行一次
构造代码块-先声明-执行
构造代码块-后声明-执行
构造器执行
```

**静态代码块**

在java中使用static关键字声明的代码块。静态块用于初始化类，为类的属性初始化。每个静态代码块**只会执行一次**。当main方法和静态代码块写在同一个文件中时，由于JVM在加载类时会执行静态代码块，所以静态代码块**先于主方法执行**。

如果类中包含多个静态代码块，那么将按照"先定义的代码先执行，后定义的代码后执行"。

注意：1 静态代码块不能存在于任何方法体内。2 静态代码块不能直接访问静态实例变量和实例方法，需要通过类的实例对象来访问。

栗子：

先编写一个父类代码

```java
public class TestClass5 {

    {
        System.out.println("父类构造代码块--先声明--执行");
    }

    {
        System.out.println("父类构造代码块--后声明--执行");
    }

    static {
        System.out.println("父类静态代码块--先声明--执行");
    }

    public TestClass5() {
        System.out.println("父类构造方法执行！");
    }

    static {
        System.out.println("父类静态代码块--后声明--执行");
    }
}
```

编写子类代码

```java
public class TestClass6 extends TestClass5 {

    public static final Integer filed = 0;

    {
        System.out.println("子类构造代码块--先声明--执行");
    }

    {
        System.out.println("子类构造代码块--后声明--执行");
    }

    static {
        System.out.println("子类静态代码块--先声明--执行");
    }

    public TestClass6() {
        System.out.println("子类构造方法执行！");
    }

    static {
        System.out.println("子类静态代码块--后声明--执行");
    }

    public static void main(String[] args) {
        Integer filed = TestClass6.filed;
        Integer filed1 = TestClass6.filed;
    }
}
```

执行main 方法输出如下：

```
父类静态代码块--先声明--执行
父类静态代码块--后声明--执行
子类静态代码块--先声明--执行
子类静态代码块--后声明--执行
```

可以看出来，父类的静态代码块先执行，之后再执行子类，由于没有实例化对象，所以没有执行构造方法和构造代码块，上面我们只是调用了一下子类中的变量，所以只要类一旦加载，静态代码块就会执行，且只执行一次。

如果实例化两个类

```java
public static void main(String[] args) {
        TestClass6 a = new TestClass6();
        TestClass6 b = new TestClass6();
    }
```

输出结果：

```java
//静态代码块最先执行，且只执行了一次
父类静态代码块--先声明--执行
父类静态代码块--后声明--执行
子类静态代码块--先声明--执行
子类静态代码块--后声明--执行

//构造代码块,实例化了两次，执行了两次
//一次执行
父类构造代码块--先声明--执行
父类构造代码块--后声明--执行
父类构造方法执行！
子类构造代码块--先声明--执行
子类构造代码块--后声明--执行
子类构造方法执行！

//二次执行，同样也是父类的构造代码块先执行
父类构造代码块--先声明--执行
父类构造代码块--后声明--执行
父类构造方法执行！
子类构造代码块--先声明--执行
子类构造代码块--后声明--执行
子类构造方法执行！
```

