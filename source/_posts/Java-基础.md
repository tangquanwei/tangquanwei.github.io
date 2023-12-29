---
title:  Java 基础 
date:  2022-01-28 20:27:32
tags: Java
---


# Java一学期复习 & 基础入门

<font color=#999AAA >
学了一学期Java了现在开始复习吧
</font>

<hr style=" border:solid; width:100px; height:1px;" color=#000000 size=1">

<!-- more -->

# 一、Java开发入门
## 1.1 Java概述
Java 是由 Sun Microsystems 公司于 1995 年 5 月推出的 Java 面向对象程序设计语言和 Java 平台的总称。
由 James Gosling和同事们共同研发，并在 1995 年正式推出。

后来 Sun 公司被 Oracle （甲骨文）公司收购，Java 也随之成为 Oracle 公司的产品。

Java分为三个体系：
	
	JavaSE (J2SE) (Java 2 Platform Standard Edition，java平台标准版）
	JavaEE (J2EE) (Java 2 Platform,Enterprise Edition，java平台企业版)
	JavaME (J2ME) (Java 2 Platform Micro Edition，java平台微型版)

## 1.2 JDK, JRE, JVM
**JDK(Java Development Kit)又称J2SDK(Java2 Software Development Kit)**

是Java开发工具包，它提供了Java的开发环境(提供了编译器javac等工具，用于将java文件编译为class文件)和运行环境(提 供了JVM和Runtime辅助包，用于解析class文件使其得到运行)。如果你下载并安装了JDK，那么你不仅可以开发Java程序，也同时拥有了运行Java程序的平台。JDK是整个Java的核心，包括了Java运行环境(JRE)，一堆Java工具tools.jar和Java标准类库 (rt.jar)。

**JRE(Java Runtime Enviroment)是Java的运行环境**

面向Java程序的使用者，而不是开发者。如果你仅下载并安装了JRE，那么你的系统只能运行Java程序。JRE是运行Java程序所必须环境的集合，包含JVM标准实现及 Java核心类库。它包括Java虚拟机、Java平台核心类和支持文件。它不包含开发工具(编译器、调试器等)。

**JVM（Java Virtual Machine）Java 虚拟机**

是整个 Java 实现跨平台的最核心的部分，能够运行以 Java 语言写作的软件程序。

# 二、Java编程基础
## 2.1 基本语法
```java
/**
 * 文档注释
 * 
 * (类注释)
 * 
 * 文件First.java
 * 
 * 主类名必修和文件名一致
 */
public class First {
    static int sCnt = 0; // 类变量
    int cnt = 0;// 实例变量
    final int SIZE = 10; // const
    static String name = "Quanwei";

    public void printArray(int[] array) {
        System.out.print(array);
    }

    public void addCnt() {
        ++sCnt;
        ++cnt;
    }

    public void showCnt() {
        System.out.println(cnt);
    }

    public static void showName(boolean isOn) {
        if (isOn)
            System.out.println(name);
    }

    public static void convert() {
        /* 英制到公制 */
        int foot;
        int inch;// 两个整数的运算结果一定是整数
        Scanner in = new Scanner(System.in);// 惯用
        foot = in.nextInt();
        inch = in.nextInt();
        System.out.println((foot + inch / 12f) * 0.3048);
        in.close();
    }

    public static void main(String[] args) {
        // showName(true);
        // int age1 = 10;// 局部变量
        convert();
    }
}
```

## 2.2 数据类型
八种基本数据类型
```java
public class Type {
    /* 基本类型 默认值 */
    byte y; // byte: 0
    short s; // short: 0
    int i; // int: 0
    long l; // long: 0
    double d; // double: 0.0
    float f; // float: 0.0
    boolean b; // boolean: false
    char c; // char:

    void show() {
        System.out.println("byte: " + y);
        System.out.println("short: " + s);
        System.out.println("int: " + i);
        System.out.println("long: " + l);
        System.out.println("double: " + d);
        System.out.println("float: " + f);
        System.out.println("boolean: " + b);
        System.out.println("char: " + c);
    }

    void assign() {
        y = -1;
        s = 1;
        i = 2;
        l = 3L;
        d = 12.5;
        f = 6.25F;
        b = true;
        c = 'c';
    }

    public static void main(String[] args) {
    	// type是引用类型
        Type type = new Type();
        type.assign();
        type.show();
    }
}
```

## 2.3 运算符
| 类型  | 运算符  |
|--|--|
| 成员访问运算符 | **.** |
| 下标运算符 | [ ] |
| 函数调用运算符 | ( ) |
| 算数运算符 | * 乘&#8195; / 除 &#8195;% 取余&#8195; + &#8195;- |
| 按位运算 | & 与&#8195;\| 或 &#8195;~ 取反 &#8195;^ 异或&#8195; << 左移&#8195; >> 右移 &#8195;>>> 无符号右移 |
| 逻辑运算 | &&#8195; \|&#8195; !非&#8195; && 短路与&#8195; \|\| 短路非 |
| 条件运算 | <&#8195;>&#8195; <= &#8195;>=&#8195; ==&#8195; != |
| 条件运算符 | ? **:** |
| 赋值运算符 | =&#8195; +=&#8195; -=&#8195; *= &#8195;/= &#8195;<<= &#8195;>>=&#8195; >>>=&#8195;&= &#8195;\|=&#8195; ^=|
## 2.4 选择结构
```java
        boolean condition=true;
        // if-else
        if (condition) {
            //todo
        }else{
			//todo
        }
        // if-else if-else
        if (condition){
            // todo
        }else if(!condition){
			//todo
        }else{
			//todo
        }
        //switch
        switch (key) {
            case value:
                //todo
                break;
        
            default:
                break;
        }
```
## 2.5 循环结构
```java
        boolean condition = true;
        while (condition) {
            // todo
            break;//跳出循环
        }

        do {
            // todo
            continue;//进入下次循环
        } while (condition);

        for (int i = 0; i < 100; i++) {
            // todo
        }
```
# 三、面向对象
<font color=#999AAA >
面向对象的优点:<br>
可重用性：代码重复使用，减少代码量，提高开发效率。<br>
可扩展性：指新的功能可以很容易地加入到系统中来，便于软件的修改。<br>
可管理性：能够将功能与数据结合，方便管理。
</font>

## 3.1 概念
Java 是面向对象的编程语言，对象就是面向对象程序设计的核心。
所谓对象就是真实世界中的实体，对象与实体是一一对应的，也就是说现实世界中每一个实体都是一个对象，它是一种具体的概念。对象有以下特点：

	 1. 对象具有属性和行为。
	 2. 对象具有变化的状态。 
	 3. 对象具有唯一性。 
	 4. 对象都是某个类别的实例。 
	 5. 一切皆为对象，真实世界中的所有事物都可以视为对象。

## 3.2 特性(封装, 继承, 多态)
### 3.2.1 封装
为什么要封装?

	 1. 保护类中的信息，它可以阻止在外部定义的代码随意访问内部代码和数据。 
	 2. 隐藏细节信息，一些不需要程序员修改和使用的信息，用户不需要知道。
	 3. 有助于建立各个系统之间的松耦合关系，提高系统的独立性。当一个系统的实现方式发生变化时，只要它的接口不变，就不会影响其他系统的使用。
	 4. 提高软件的复用率，降低成本。每个系统都是一个相对独立的整体，可以在不同的环境中得到使用

怎样封装? (将类中属性设为私有, 公开获取属性值的接口)
```java
public class User {
    /* 私有属性 */
    private int id;
    private String username;
    private String passpord;

    /**
     * 构造函数
     */
    public User() {
    }

    /**
     * @return the passpord
     */
    public String getPasspord() {
        return passpord;
    }

    /**
     * @param passpord the passpord to set
     */
    public void setPasspord(String passpord) {
        this.passpord = passpord;
    }

    /**
     * @return the id
     */
    public int getId() {
        return id;
    }

    /**
     * @param id the id to set
     */
    public void setId(int id) {
        this.id = id;
    }

    /**
     * @return the username
     */
    public String getUsername() {
        return username;
    }

    /**
     * @param username the username to set
     */
    public void setUsername(String username) {
        this.username = username;
    }
        
   @Override
   public String toString() {
       return "id: "+getId()+"\tusername: "+getUsername()+"\tpassword: "+getPasspord();
   }
}
```
### 访问修饰符: 

 **default (即默认，什么也不写）:** 

	在同一包内可见，不使用任何修饰符。
使用对象：类、接口、变量、方法。

**public :** 

	对所有类可见。
使用对象：类、接口、变量、方法 protected : 对同一包内的类和所有子类可见。使用对象：变量、方法。

**private :** 

	在同一类内可见。
使用对象：变量、方法。 注意：不能修饰类（外部类）

**protected :** 

	对同一包内的类和所有子类可见。
使用对象：变量、方法。 注意：不能修饰类（外部类）

注意：

父类中声明为 public 的方法在子类中也必须为 public。 父类中声明为 protected的方法在子类中要么声明为 protected，要么声明为 public，不能声明为 private 父类中声明为 private的方法，不能够被继承

### 3.2.2 继承
什么是继承?

	程序中的继承性是指子类拥有父类的全部特征和行为，这是类之间的一种关系。Java 只支持单继承。

如何继承? (使用关键字 **extends**)
```java
public class VipUser extends User {
    public static void main(String[] args) {
        VipUser vip = new VipUser();
        vip.setUsername("Quanwei");
        System.out.println(vip.getUsername());// Quanwei
    }
}
```
### 3.2.3 多态
什么是多态?

	面向对象的多态性，即“一个接口，多个方法”。

为什么要使用多态?

	多态性体现在父类中定义的属性和方法被子类继承后，可以具有不同的属性或表现方式。
	多态性允许一个接口被多个同类使用，弥补了单继承的不足。

如何使用多态? ( 重写父类方法)
```java
package lang.aboutclass;

public class VipUser extends User {
    private int vipNum;

    @Override
    public String toString() {
        return super.toString() + "\tvipNum: " + getVipNum();
    }

    /**
     * @return the vipNum
     */
    public int getVipNum() {
        return vipNum;
    }

    /**
     * @param vipNum the vipNum to set
     */
    public void setVipNum(int vipNum) {
        this.vipNum = vipNum;
    }

    public static void main(String[] args) {
        User user = new VipUser();
        user.setUsername("Quanwei");
        if (user instanceof VipUser vip) {
            vip.setVipNum(5432);
            System.out.println(vip.getUsername());// Quanwei
            System.out.println(vip);
        }
    }
}

```
### 非访问修饰符
**static 修饰符**
静态变量：
static 关键字用来声明独立于对象的静态变量，无论一个类实例化多少对象，它的静态变量只有一份拷贝。 静态变量也被称为类变量。局部变量不能被声明为 static 变量。

静态方法：
static 关键字用来声明独立于对象的静态方法。静态方法不能使用类的非静态变量。静态方法从参数列表得到数据，然后计算这些数据。

**final 修饰符**
final 变量:
final 表示"最后的、最终的"含义，变量一旦赋值后，不能被重新赋值。被 final 修饰的实例变量必须显式指定初始值。
final 修饰符通常和 static 修饰符一起使用来创建类常量。

final 方法:
父类中的 final 方法可以被子类继承，但是不能被子类重写。

final 类:
final 类不能被继承，没有类能够继承 final 类的任何特性。

**abstract 修饰符**
抽象类：
抽象类不能用来实例化对象，声明抽象类的唯一目的是为了将来对该类进行扩充。
一个类不能同时被 abstract 和 final 修饰。如果一个类包含抽象方法，那么该类一定要声明为抽象类，否则将出现编译错误。
抽象类可以包含抽象方法和非抽象方法。

抽象方法
抽象方法是一种没有任何实现的方法，该方法的的具体实现由子类提供。
抽象方法不能被声明成 final 和 static。
任何继承抽象类的子类必须实现父类的所有抽象方法，除非该子类也是抽象类。
如果一个类包含若干个抽象方法，那么该类必须声明为抽象类。抽象类可以不包含抽象方法。
抽象方法的声明以分号结尾，例如：public abstract sample();。

## 3.3 特殊类
### 3.3.1 抽象类 ( Abstract Class)

在面向对象的概念中，所有的对象都是通过类来描绘的，但是反过来，并不是所有的类都是用来描绘对象的，如果一个类中没有包含足够的信息来描绘一个具体的对象，这样的类就是抽象类。
抽象类不能用于实例化对象

如何定义? (使用**abstract**关键字)
```java
// 抽象类
public abstract class Human {
    String name;
    int age;
    String language;

    // 抽象方法
    public abstract void say();
}
```

如何使用? (**继承**)
```java
class Chinese extends Human {
    Chinese(String name, String language) {
        this.name = name;
        this.language = language;
    }

    @Override
    public void say() {
        System.out.println(name + " says " + language);
    }
}
```
### 3.3.2 接口 (Interface)
是一个抽象类型，是抽象方法的集合。一个类通过继承接口的方式，从而来继承接口的抽象方法。
接口并不是类，类描述对象的属性和方法。接口则包含类要实现的方法。
除非实现接口的类是抽象类，否则该类要定义接口中的所有方法。
接口无法被实例化，但是可以被实现。一个实现接口的类，必须实现接口内所描述的所有方法。

如何定义? (使用interface关键字)
```java
/**
 1. 可以跑的接口 :)
 */
public interface Runnable {
    // 跑的方法
    void run();
}
```
如何使用? (使用implements关键字实现接口)
```java
class Chinese extends Human implements Runnable{
    Chinese(String name, String language) {
        this.name = name;
        this.language = language;
    }

    @Override
    public void say() {
        System.out.println(name + " says " + language);

    }
    /**
     * 实现接口须重写run方法
     */
    @Override
    public void run() {
       System.out.println("I am Running...");        
    }
}
```
### 3.3.3 内部类 ( Inner Class )
Java 一个类中可以嵌套另外一个类, 即内部类
如何定义?
```java
class OuterClass {
    String infomation = "Outer class";

    class InnerClass {
        String information = "Inner class";

        class InsiderClass {
            String information = "Insider class";

        }
    }
}
```
如何使用? ( 外部类.new 内部类名() )
```java
package lang.aboutclass;

import lang.aboutclass.OuterClass.InnerClass;
import lang.aboutclass.OuterClass.InnerClass.InsiderClass;

class Main {
    public static void main(String[] args) {
        OuterClass outer = new OuterClass();
        InnerClass inner = outer.new InnerClass();
        InsiderClass insider = inner.new InsiderClass();
        System.out.println(outer.infomation);// Outer class
        System.out.println(inner.information);// Inner class
        System.out.println(insider.information);// Insider class
    }
}
```
### 3.3.4 枚举类 ( Enum )
Java 枚举是一个特殊的类，一般表示一组常量
如何定义? (使用 enum 关键字来定义，各个常量使用逗号 **,** 来分割)
```java
public enum Weekday {
    SUN, MON, TUE, WED, THU, FRI, SAT;
}
```
如何使用? (枚举类名.常量)
```java
System.out.println(Weekday.FRI);//FRI
```

### 3.3.6 记录类
```java
// 定义一个Point类，有x、y两个变量，同时它是一个不变类
public record Point(int x, int y) {
}
```
相当于:
```java
public final class Point extends Record {

private final int x;

private final int y;

public Point(int x, int y) { this.x = x; this.y = y; }

public int x() { return this.x; }

public int y() { return this.y; }

public String toString() { return String.format("Point[x=%s, y=%s]", x, y); }

public boolean equals(Object o) { //... } public int hashCode() { //... } }
```
也可以在里面写构造函数
```java
public record Point(int x, int y) {
    // Compact Constructor，它的目的是让我们编写检查逻辑 
    public Point {
        if (x < 0 || y < 0) {
            throw new IllegalArgumentException();
        }
    }
}
```

### 3.3.6 注解类（Annotation）
Java 语言中的类、方法、变量、参数和包等都可以被标注。
和 Javadoc 不同，Java 标注可以通过反射获取标注内容。在编译器生成类文件时，标注可以被嵌入到字节码中。Java 虚拟机可以保留标注内容，在运行时可以获取到标注内容 。 当然它也支持自定义 Java 标注。

如何定义? ( 使用@interface关键字 )
```java
package lang.annotation;

import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

// 3.用元注解配置注解 
// 必须设置@Target和@Retention，@Retention一般设置为RUNTIME，因为我们自定义的注解通常要求在运行期读取
@Target(ElementType.LOCAL_VARIABLE)
@Retention(RetentionPolicy.RUNTIME)
public @interface Report {// 1.用@interface定义注解
    // 2.添加默认参数
    int type() default 0;

    String level() default "info";

    String value() default "";
    // 把最常用的参数定义为value()，推荐所有参数都尽量设置默认值
}
// 注解定义后也是一种class，所有的注解都继承自java.lang.annotation.Annotation，因此，读取注解，需要使用反射API
```
**元注解（meta annotation）** 有一些注解可以修饰其他注解，这些注解就称为元注解

最常用的元注解是@Target。
使用@Target可以定义Annotation能够被应用于源码的哪些位置：
类或接口：ElementType.TYPE；
字段：ElementType.FIELD；
方法：ElementType.METHOD；
构造方法：ElementType.CONSTRUCTOR；
方法参数：ElementType.PARAMETER

另一个重要的元注解@Retention定义了Annotation的生命周期：
仅编译期：RetentionPolicy.SOURCE；
仅class文件：RetentionPolicy.CLASS； (默认)
运行期：RetentionPolicy.RUNTIME

如何使用? ( 通过反射获取注解信息 )
```java
@MyAnnotation(value = 20, name = "Quanwei")
public class MyAnnoTest {

    public static void main(String[] args) {
        MyAnnotation anno = MyAnnoTest.class.getDeclaredAnnotation(MyAnnotation.class);
        System.out.println(anno.name());// Quanwei
        System.out.println(anno.value());// 20
    }
}
```
在注解的定义中使用注解:
```java
@Target({ ElementType.TYPE, ElementType.FIELD, ElementType.METHOD })//注解使用位置
@Retention(RetentionPolicy.RUNTIME)// 保留时间
public @interface MyAnnotation {
    int value() default 12;

    String name() default "quanwei";

    Report report()default @Report ;
}
```

## 3.4 反射 Reflection
是什么?

	反射就是一种能在程序 运行 时获取类、接口、方法和变量等信息的能力。
有什么用?
	
	解决在运行期，对某个实例一无所知的情况下，调用其方法
如何使用? (类名.class)
```java
public class Main {
    public static void main(String[] args) throws Exception {
        Class<Student> stdClass = Student.class;
        // 获取public方法getScore，参数为String:
        System.out.println(stdClass.getMethod("getScore", String.class));
        // 获取继承的public方法getName，无参数:
        System.out.println(stdClass.getMethod("getName"));
        // 获取private方法getGrade，参数为int:
        System.out.println(stdClass.getDeclaredMethod("getGrade", int.class));
    }
}

class Student extends Person {
    public int getScore(String type) {
        return 99;
    }
	@SuppressWarnings("unused")
    private int getGrade(int year) {
        return 1;
    }
}

class Person {
    public String getName() {
        return "Person";
    }
}

```
还有多种情况,[参考](https://www.oracle.com/technical-resources/articles/java/javareflection.html)
## 3.5 lambda表达式
lambda表达式也即**匿名函数**, 允许把函数作为一个方法的参数（函数作为参数传递进方法中）
在java中lambda表达式本质上是一个对象

基本的语法像这样:
	
	(parameters) ->{ statements; }

基本使用
```java
new Thread(() -> {
            try {
                Thread.sleep(1000);
            } catch (Exception e) {
                e.printStackTrace();
            }
        }).start();
```
## 3.6 异常
异常主要指程序运行的时候，会发生各种错误, 不包括编译时发生的

异常可以捕获:
```java
		try{
            int i=10/0;
        }catch(Exception e){
            e.printStackTrace();
        }finally{
           // todo
        }
```
也可以抛出
```java	
	// 自动抛出
	 public static void test() throws IOException {
        testException(10,0);
    }

	// 手动抛出
    public static void testException(int a, int b) throws RuntimeException {
        if (b > 0) {
            System.out.println(a / b);
        } else {
            throw new RuntimeException("除数不能为零");
        }
    }
    
```
还可以自定义异常 (继承Exception类)
```java
public class MyException extends Exception {
    public MyException(String msg) {
        super(msg+" from MyException");
    }
}
```
# 四、常用类
## 4.1 数组 & Arrays
数组的定义和使用
```java
        // 1.创建array对象
        int[] array=new int[10];
        // 赋值
        for (int i = 0; i < array.length; i++) {
            array[i]=i+1;
        }
        // 使用
        for (int i : array) {
            System.out.print(i+" ");
        }
        // 其他
        int[] a2={1,2,2,3,4};
        String[] strings={"asd","sdas"};
```
Arrays 类是一个工具类，其中包含了数组操作的很多方法。 Arrays 类里均为 static 修饰的方法可以直接通过类名调用。
例如使用Arrays对数组排序
```java
        // 1.创建array对象, 并赋值
        String[] strings = { "hello", "thank", "you", "are", "ok" };
        // 顺序
        Arrays.sort(strings);
        System.out.println(Arrays.toString(strings)); // [are, hello, ok, thank, you]
        // 逆序
        Arrays.sort(strings, Comparator.reverseOrder());
        System.out.println(Arrays.toString(strings)); // [you, thank, ok, hello, are]
```
## 4.2 String, StringBuffer和StringBuilder
### 4.2.1 String
 String 是 final 修饰的，无法被继承。且 String 不是 Java 的基本数据类型
通过new创建的String对象,和直接赋值的String对象稍有不同:
```java
/**
 * TestString
 * 
 * 可以和八种基本数据类型做运算
 */
public class TestString {

    public static void main(String[] args) {

    }
    public void testStringJoint() {
        // 字面量定义,s1指向方法区中的静态常量池
        String s1 = "Quanwei";
        String s2 = "Quanwei";
        // s2->堆空间地址值
        String s3 = new String("Quanwei");
        System.out.println("s1 == s2: " + (s1 == s2)); // true
        System.out.println("s1 == s3: " + (s1 == s3)); // false
        System.out.println("s1.equals(s2): " + (s1.equals(s2)));// true
        System.out.println("s1.equals(s3): " + (s1.equals(s3)));// true

        String s4 = "TangQuanwei";
        String s5 = "Tang" + "Quanwei"; // 常量与常量拼接在静态常量池
        System.out.println("s4 == s5: " + (s4 == s5)); // true
        String s6 = "Tang" + s1; // 常量与引用拼接->堆空间地址值
        System.out.println("s4 == s6: " + (s4 == s6)); // false
    }

 
    public void testToCharArray() {
        String s1 = "Quanwei";
        char[] charArray = s1.toCharArray();
        System.out.println(Arrays.toString(charArray));

        String s2 = "权威";
        char[] charArray2 = s2.toCharArray();
        System.out.println(Arrays.toString(charArray2));

    }

  
    public void testGetBytes() {
        String s1 = "Quanwei";
        byte[] bytes = s1.getBytes();
        System.out.println(Arrays.toString(bytes));

        String s2 = "权威";
        byte[] bytes2 = s2.getBytes();// 默认utf-8
        System.out.println(Arrays.toString(bytes2));

        try {
            byte[] bytes3 = s2.getBytes("gbk");
            System.out.println(Arrays.toString(bytes3));

            byte[] bytes4 = s2.getBytes("utf-8");
            System.out.println(Arrays.toString(bytes4));
        } catch (UnsupportedEncodingException e) {
            e.printStackTrace();
        }
    }
}
```

 String 是不可变的，而 StringBuffer 和 StringBuilder 是可变类
 
 ### 4.2.2 StringBuffer和StringBuilder
 二者区别就是:
 | StringBuffer | StringBuilder |
 |--|--|
|线程安全  |	非线程安全 |
|同步	|非同步|
|慢|	快|
所以在非多线程环境中的字符串操作，我们一般用 StringBuilder , 因为二者的方法都是差不多的
它们为字符串操作提供了 append、insert、delete 和 substring 方法
```java
    public void testStringBuilder() {
        var sb1 = new StringBuilder("Tang");
        var sb2 = new StringBuilder("t");
        System.out.println(sb1.capacity());// 20
        System.out.println(sb1.length());// 4
        sb1.ensureCapacity(40);
        System.out.println(sb1.capacity());// 42
        System.out.println(sb2.capacity());// 17
        sb1.append("Quawnei");
        System.out.println(sb1);//TangQuawnei
        sb1.insert(4, "->");
        System.out.println(sb1);//Tang->Quawnei
        sb1.delete(0, 4);
        System.out.println(sb1);//->Quawnei
    }
```

## 4.3 System类和Runtime类
### 4.3.1 静态字段
System 类有 3 个静态成员变量，分别是 PrintStream **out**、InputStream **in** 和 PrintStream **err**
out跟err的区别大概是:
System.out.println可能会被缓冲,而System.err.println不会，由于err不需要缓冲即可输出
	
	PrintStream out
```java
System.out.println("quanwei);
```
	InputStream in 
```java
    try {
        int read = System.in.read();// 读一个字符的ascii码
    } catch (IOException e) {
        e.printStackTrace();
    }
```
### 4.3.2 静态方法
```java
public static void main(String[] args) {
        // 获取系统属性
        String jversion = System.getProperty("java.version");
        String oName = System.getProperty("os.name");
        String user = System.getProperty("user.name");
        System.out.println("Java 运行时环境版本：" + jversion);// Java 运行时环境版本：17.0.1
        System.out.println("当前操作系统是：" + oName);// 当前操作系统是：Windows 10
        System.out.println("当前用户是：" + user);// 当前用户是：QUANWEI

        // 时间的格式为当前计算机时间与 GMT 时间（格林尼治时间）
        // 1970 年 1 月 1 日 0 时 0 分 0 秒所差的毫秒数
        Date date = new Date(System.currentTimeMillis());//
        System.out.println(date); // Mon Jan 10 20:20:43 CST 2022

        int[] a = { 1, 2, 3, 4, 5 };
        int[] b = { 2, 3, 4, 5, 6 };
        // 合并数组
        int[] marge = marge(a, b);
        System.out.println(Arrays.toString(marge));// [1, 2, 3, 4, 5, 2, 3, 4, 5, 6]

        // 请求系统进行垃圾回收
        System.gc();

        // 终止当前正在运行的 Java 虚拟机
        System.exit(0);
    }

    public static int[] marge(int[] a, int[] b) {
        int[] ret = new int[a.length + b.length];
        System.arraycopy(a, 0, ret, 0, a.length);
        System.arraycopy(b, 0, ret, a.length, b.length);
        return ret;
    }
```
## 4.4 包装类
| 基本类 | 包装类 |
|-|-|-|
|byte| Byte|
|short | Short|
|int | Integer|
|long | Long|
|char | Character|
|float| Float|
|double | Double|
|boolean | Boolean|
常用方法 :
		**包装类名.parseXxx(String );**
		**包装类名.valueOf(String );**
		**包装类名.toString();**
## 4.5 Math和Random
Math类就是用来进行数学计算的，它提供了大量的静态方法来便于我们实现数学计算
比如:
```java
// 数学常量：
double pi = Math.PI; // 3.14159...
double e = Math.E; // 2.7182818...

// 求绝对值：
Math.abs(-100); // 100
Math.abs(-7.8); // 7.8

// 取最大或最小值：

Math.max(100, 99); // 100
Math.min(1.2, 2.3); // 1.2

// 计算xy次方：
Math.pow(2, 10); // 2的10次方=1024

// 计算√x：
Math.sqrt(2); // 1.414...

// 计算ex次方：
Math.exp(2); // 7.389...

// 计算以e为底的对数：
Math.log(4); // 1.386...

// 计算以10为底的对数：
Math.log10(100); // 2

// 三角函数：
Math.sin(3.14); // 0.00159...
Math.cos(3.14); // -0.9999...
Math.tan(3.14); // -0.0015...
Math.asin(1.0); // 1.57079...
Math.acos(1.0); // 0.0
```

## 4.6 BigInteger类和BigDecimal类
<font color=#999AAA >
在Java中，由CPU原生提供的整型最大范围是64位long型整数。使用long型整数可以直接通过CPU指令进行计算, 如果我们使用的整数范围超过了long型就只能用软件来模拟一个大整数。java.math.BigInteger就是用来表示任意大小的整数。BigInteger内部用一个int[]数组来模拟一个非常大的整数
</font>
<br />
<br />
由于Java不支持运算符重载, 在对BigInteger做运算的时候，只能使用对象的方法，例如，

```java
BigInteger i1 = new BigInteger("1234567890");
BigInteger i2 = new BigInteger("12345678901234567890");
BigInteger sum = i1.add(i2); // 12345678902469135780
```
BigDecimal和BigInteger类似，可以用来表示一个任意大小且精度完全准确的浮点数
```java
BigDecimal d1 = new BigDecimal("9876543210.0123456789");
BigDecimal d2 = new BigDecimal("123.45000000000000000");
System.out.println(d1.scale()); // 10,两位小数
System.out.println(d1.add(d2));
System.out.println(d1.subtract(d2));
System.out.println(d1.multiply(d2));
// 无法除尽时，就必须指定精度以及如何进行截断
System.out.println(d1.divide(d2, 10, RoundingMode.HALF_UP));// 四舍五入
```
## 4.7 时间和日期类
JDK8以前
```java
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Calendar;
import java.util.Date;
/**
 * JDK 8 以前 时间日期API
 * 
 * 1.System.currentTimeMillis()
 * 
 * 2.java.util.Calendar;  java.util.Date;
 * 
 * 3.SimpleDateFormat
 * 
 * 4.Calender
 */
public class TestBefer8 {
    public void testTime() {
        // 获取当前时间的毫秒数时间戳
        System.out.println("TestDateTime.testTime()");
        long currentTimeMillis = System.currentTimeMillis();
        System.out.println(currentTimeMillis);
    }
    public void testDate() {
        // java.util.Date
        Date d1 = new Date();
        System.out.println(d1);
        System.out.println(d1.getTime());// 毫秒数时间戳

        var d2 = new Date(1631504736660L);
        System.out.println(d2);

        // java.sql.Date 是 java.util.Date 的子类
        var d3 = new java.sql.Date(0L);
        System.out.println(d3);

        // java.sql.Date -> java.util.Date
        Date d4 = (Date) d3;
        System.out.println(d4);

        // java.util.Date -> java.sql.Date
        java.sql.Date d5 = new java.sql.Date(d4.getTime());
        System.out.println(d5);
    }

    /**
     * SimpleDateFormat对Date类的格式化和解析
     * 
     * 1.格式化: 日期 --> 字符串
     * 
     * 2.解析:格式化的逆过程: 字符串 --> 日期
     */
    public void testFormat() {
        System.out.println("TestDateTime.testFormat()");

        // 格式化
        SimpleDateFormat sdf = new SimpleDateFormat();
        Date date = new Date();
        String format = sdf.format(date);
        System.out.println(format);

        // 指定格式
        final String parttern = "yyyy-MM-dd hh:mm:ss:SSS z";
        SimpleDateFormat sdf2 = new SimpleDateFormat(parttern);
        System.out.println(sdf2.format(date));

        try {
            // 解析
            String str = "2021/9/13 下午2:58";
            String s2 = "2002-10-09 08:31:12:234 CST";
            Date d1 = sdf.parse(str);
            System.out.println(d1);
        } catch (ParseException e) {
            e.printStackTrace();
        }
    }

    /**
     * Calender 抽象基类
     * 
     * 主要用于完成日期字段之间相互操作的功能
     * 
     */
    public void testCalender() {
        // 1.调用静态方法获取实例
        Calendar cal = Calendar.getInstance();
        Calendar instance2 = Calendar.getInstance();
        System.out.println(cal.getClass());// class java.util.GregorianCalendar
        System.out.println(cal.equals(instance2));//false
        
        //get
        System.out.println(cal.get(Calendar.DAY_OF_MONTH));
        System.out.println(cal.get(Calendar.DAY_OF_YEAR));
    }
}
```
JDK8以后
```java
import java.time.Instant;
import java.time.LocalDate;
import java.time.LocalDateTime;
import java.time.LocalTime;
import java.time.OffsetDateTime;
import java.time.ZoneOffset;
import java.time.format.DateTimeFormatter;
import java.time.format.FormatStyle;
import java.time.temporal.TemporalAccessor;
/**
 * JDK 8 中的时间日期API
 * 
 * 1.LocalDate
 * 
 * 2.LocalTime
 * 
 * 3.LocalDateTime
 * 
 * 4.Instant 机器时间
 * 
 * 5.DateTimeFormater
 */
public class TestIn8 {
    public void testDate() {
        LocalDate now = LocalDate.now();
        System.out.println(now);
    }

    public void testTime() {
        LocalTime now = LocalTime.now();
        System.out.println(now);
    }

    public void testLocalDateTime() {
        // now 获取当前的日期和时间
        LocalDateTime now = LocalDateTime.now();
        System.out.println(now);

        // of 获取指定的日期时间 没有偏移量
        LocalDateTime of = LocalDateTime.of(2002, 10, 9, 16, 34, 35);
        System.out.println(of);

        // getXxx
        System.out.println(now.getDayOfMonth());
        System.out.println(now.getDayOfYear());
        System.out.println(now.getChronology());
    }

    public void testInstant() {
        // now() 获取本初子午线对应的标准时间
        Instant now = Instant.now();
        System.out.println(now);// 慢八个小时

        // 添加时间偏移量
        OffsetDateTime offSet = now.atOffset(ZoneOffset.ofHours(8));
        System.out.println(offSet);

        // 1970(UTC)->现在 经过的毫秒数
        long epochMilli = now.toEpochMilli();
        System.out.println(epochMilli);// 1631521135573

        // 毫秒数->instant
        Instant ofEpochMilli = Instant.ofEpochMilli(1631521135573L);
        System.out.println(ofEpochMilli);
    }

    public void testDateTimeFormater() {
        LocalDateTime now = LocalDateTime.now();

        // 格式化1
        DateTimeFormatter formatter = DateTimeFormatter.ISO_LOCAL_DATE_TIME;
        String format = formatter.format(now);
        System.out.println(format); // 2021-09-13T16:56:14.4738222

        // 解析
        TemporalAccessor parse = formatter.parse("2021-09-13T16:56:14.4738222");
        System.out.println(parse);

        // 格式化2: 本地格式化
        DateTimeFormatter formatter2 = DateTimeFormatter.ofLocalizedDateTime(FormatStyle.MEDIUM);
        String format2 = formatter2.format(now);
        System.out.println(format2);

        // 格式化3: 自定义格式化
        final String pattern="GG yyyy年MM月dd日 EE a hh时mm分ss秒";
        DateTimeFormatter formatter3 = DateTimeFormatter.ofPattern(pattern);
        String format3 = formatter3.format(LocalDateTime.now());
        System.out.println(format3);
    }
}

```
# 五、集合
<font color=#999AAA >
Java Collection Framework  (JCF) Java集合框架是一组提供现成架构的类和接口。为了实现一个新特性或一个类，不需要定义一个框架。然而，一个最佳的面向对象设计总是包含一个具有类集合的框架，这样所有的类都执行相同类型的任务。
</font>

## 5.1 Collection和Collections
Collections
```java
    public static void main(String[] args) {
        List<String> ls = new ArrayList<>();
        ls.add("reverse");
        ls.add("shuffle");
        ls.add("sort");
        ls.add("swap");
        ls.add("rotate");
        System.out.println(ls);// [reverse, shuffle, sort, swap, rotate]

        // 反转
        Collections.reverse(ls);
        System.out.println(ls);// [rotate, swap, sort, shuffle, reverse]

        // 按升序进行排序
        Collections.sort(ls);
        System.out.println(ls);// [reverse, rotate, shuffle, sort, swap]

        // 根据指定 Comparator排序
        Collections.sort(ls, new Comparator<String>() {

            @Override
            public int compare(String o1, String o2) {
                return -o1.compareTo(o2);
            }

        });
        System.out.println(ls);// [swap, sort, shuffle, rotate, reverse]

        // 按下标交换
        Collections.swap(ls, 0, ls.size() - 1);
        System.out.println(ls);// [reverse, sort, shuffle, rotate, swap]

        // 将 list 集合的后 distance 个元素整体移到前面
        Collections.rotate(ls, 2);
        System.out.println(ls);// [rotate, swap, reverse, sort, shuffle]

        // 将 list 集合的前 distance 个元素整体移到后面
        Collections.rotate(ls, -3);
        System.out.println(ls);// [sort, shuffle, rotate, swap, reverse]

        // 返回指定集合中指定元素的出现次数
        ls.add("sort");
        int frequency = Collections.frequency(ls, "sort");
        System.out.println(frequency);// 2

        // 将指定集合中的所有元素复制到另一个集合中,目标集合的长度至少和源集合的长度相同
        List<String> l2 = new LinkedList<>();// ArrayList -> LinkedList
        l2.add("reverse1");
        l2.add("shuffle1");
        l2.add("sort1");
        l2.add("swap1");
        l2.add("rotate1");
        Collections.copy(ls, l2);
        System.out.println(ls);// [reverse1, shuffle1, sort1, swap1, rotate1, sort]

        // LinkedList -> ArrayList
        // ! IndexOutOfBoundsException Source does not fit in dest
        Collections.copy(l2, ls);
        System.out.println(ls);
	}

```
## 5.2 常用类, 接口
```java
    /**
     * 列表
     */
    public void testList() {
        // <> 中的类型只能是引用类型
        List<Integer> al = new ArrayList<>();
        List<Integer> ll = new LinkedList<>();
    }

    /**
     * 队列
     */

    public void testQueue() {
        Queue<Integer> ad = new ArrayDeque<>();
        Queue<Integer> ll = new LinkedList<>();
        Queue<Integer> pq = new PriorityQueue<>();

    }

    /**
     * 集合
     */
    public void testSet() {
        Set<Object> hs = new HashSet<>();
        Set<Object> ts = new TreeSet<>();

    }

    /**
     * 映射
     */
    public void testMap() {
        Map<Integer, String> hm = new HashMap<>();
        Map<Integer, String> tm = new TreeMap<>();
    }
 ```
## 5.5 Stream API
在 Java 8 中引入的 Stream API 用于处理对象的集合。流是支持各种方法的对象序列，这些方法可以流水线化以产生所需的结果。
Java 流的特点是:

流不是数据结构，而是从集合、数组或 I/O 通道获取输入。
流不会更改原始数据结构，它们仅根据流水线方法提供结果。
每个中间操作都被延迟执行并返回一个流作为结果，因此可以对各种中间操作进行流水线化。终端操作标记流的结束并返回结果。
```java
    public void testStream() {
        // 创建
        List<Integer> number = Arrays.asList(2, 3, 4, 5);

        // 使用map将number的元素平方
        List<Integer> square = number.stream().map(x -> x * x).collect(Collectors.toList());
        System.out.println(square);// [4, 9, 16, 25]

        // 创建
        List<String> names = Arrays.asList("Reflection", "Collection", "Stream");

        // 过滤
        List<String> result = names.stream().filter(s -> s.startsWith("S")).collect(Collectors.toList());
        System.out.println(result);// [Stream]

        // 排序
        List<String> show = names.stream().sorted().collect(Collectors.toList());
        System.out.println(show);// [Collection, Reflection, Stream]

        // 创建
        List<Integer> numbers = Arrays.asList(2, 3, 4, 5, 2);

        // 收集
        Set<Integer> squareSet = numbers.stream().map(x -> x * x).collect(Collectors.toSet());
        System.out.println(squareSet);// [16, 4, 9, 25]

        // forEach
        number.stream().map(x -> x * x).forEach(y -> System.out.print(y + " "));// 4 9 16 25

        // reduce
        int even = number.stream().filter(x -> x % 2 == 0).reduce(0, (ans, i) -> ans + i);// 0+2+4

        System.out.println(even);// 6
    }
```
## 5.6 泛型
<font color=#999AAA >
泛型意味着参数化类型。这个想法是允许类型（整数、字符串等，以及用户定义的类型）作为方法、类和接口的参数。使用泛型，可以创建使用不同数据类型的类。
对参数化类型进行操作的实体（例如类、接口或方法）称为泛型实体。
</font>

与 C++ 一样，我们使用 <> 来指定泛型类创建中的参数类型。要创建泛型类的对象
```java
class Pair<T, U> {
    private T first;
    private U second;

    public Pair(T first, U second) {
        this.setFirst(first);
        this.setSecond(second);
    }

    /**
     * @return the second
     */
    public U getSecond() {
        return second;
    }

    /**
     * @param second the second to set
     */
    public void setSecond(U second) {
        this.second = second;
    }

    /**
     * @return the first
     */
    public T getFirst() {
        return first;
    }

    /**
     * @param first the first to set
     */
    public void setFirst(T first) {
        this.first = first;
    }

    @Override
    public String toString() {
        return "( " + first + ", " + second + ")";
    }
}
```
# 六、I/O流
<font color=#999AAA >
IO是指Input/Output，即以内存为中心的输入和输出。
</font>

## 6.1 BIO
<font color=#999AAA >
BIO即 Blocking Input/Output, 即阻塞的I/O
</font>

### 6.1.1 Flie
用于文件和目录的创建、文件的查找和文件的删除等
```java
package lang.io;

import java.io.File;
import java.io.IOException;

import org.junit.jupiter.api.Test;


/**
 * File表示一个文件或一个文件夹 在java.io包下
 */
public class TestFile {
    // 构造一个File对象，并不会导致任何磁盘操作
    // 文件夹
    File floder = new File("D:\\workspaceFolder\\CODE_JAVA");
    // 文件
    File file = new File("D:\\workspaceFolder\\CODE_JAVA\\a.txt");

    @Test
    public void testGet() {
        // 返回绝对路径，
        System.out.println(file);
        // 返回构造方法传入的路径，
        System.out.println(file.getPath());
        // 返回绝对路径，
        System.out.println(file.getAbsolutePath());
        try { // 返回绝对路径，
            System.out.println(file.getCanonicalPath());
        } catch (IOException e) {
            e.printStackTrace();
        }

        // 输出文件夹下文件
        for (var i : floder.listFiles()) {
            System.out.println(i);
        }
    }

    @Test
    public void testCreate() {
        File f = new File("test.file");
        if (!f.exists()) {
            try {
                // 创建
                f.createNewFile();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }

    @Test
    public void testDelete() {
        File f = new File("test.file");
        if (f.exists()) {
            // 删除
            boolean delete = f.delete();
            System.out.println(delete?"delete":"not exist");
        }
    }

}

```
### 6.1.2 ByteStream
即字节流, InputStream和OutputStream都是抽象类在里面分别定义了read()和write()方法
我们使用它们的子类, FileInputStream和FileOutputStream
```java
package lang.io;

import java.io.File;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;

import org.junit.Test;

public class TestByteStream {

    public void copyPng() {
        File file = new File("D:\\workspaceFolder\\CODE_JAVA\\猫咪.png");
        File fileB = new File("D:\\workspaceFolder\\CODE_JAVA\\猫咪2.png");
        FileInputStream fileInputStream = null;
        FileOutputStream fileOutputStream = null;
        try {
            fileInputStream = new FileInputStream(file);
            fileOutputStream = new FileOutputStream(fileB);

            int len;
            byte[] bbuf = new byte[10];
            while ((len = fileInputStream.read(bbuf)) != -1) {
                fileOutputStream.write(bbuf, 0, len);
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                if (fileInputStream != null)
                    fileInputStream.close();
                if (fileOutputStream != null)
                    fileOutputStream.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }

    @Test
    public void read() {
        File file = new File("D:\\workspaceFolder\\CODE_JAVA\\lang\\io\\TestByteStream.java");
        FileInputStream fis = null;
        try {
            fis = new FileInputStream(file);
            int data;
            while ((data = fis.read()) != -1) {
                System.out.print((char) data);
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (fis != null) {
                try {
                    fis.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }

    }

    public static void main(String[] args) {
        new TestByteStream().copyPng();
    }
}

```
### 6.1.3 CharStream
CharStream即字符流

Reader是Java的IO库提供的另一个输入流接口。和InputStream的区别是，InputStream是一个字节流，即以byte为单位读取，而Reader是一个字符流，即以char为单位读取

我们使用Reader的子类FileReader来演示向文件写入数据和从文件读出数据
```java
package lang.io;

import java.io.File;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;

import org.junit.Test;

/**
 * 内存的角度
 * 
 * 输入:外部数据->内存
 * 
 * 输出:内存->外部数据
 * 
 * 字节流(8bit): InputStream, OutPutStream
 * 
 * 字符流(16bit): Reader, Writer
 */
public class TestCharStream {
    // 1.File对象指明文件
    private final File file = new File("data.txt");

    /**
     * Read from file and print to console
     */
    public void Read() {
        // 2.Reader对象
        FileReader fileReader = null;
        try {
            // 3.读
            fileReader = new FileReader(file);// 读入的文件一定要存在
            int read;
            while ((read = fileReader.read()) != -1)
                System.out.print((char) read);
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            // 4.关闭资源
            if (fileReader != null)
                try {
                    fileReader.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
        }
    }

    /**
     * Write String to file
     * 
     * @param toBeWrite
     */
    public void Write(String toBeWrite) {
        FileWriter fileWriter = null;
        try {
            fileWriter = new FileWriter(file);
            fileWriter.write("By_Quanwei\n");
            fileWriter.append(toBeWrite);
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (fileWriter != null)
                try {
                    fileWriter.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }

        }
    }

    /**
     * Copy fileA to fileB
     */
    public void copyFile(String fileA, String fileB) {
        FileReader fileReader = null;
        FileWriter fileWriter = null;
        try {
            fileReader = new FileReader(fileA);
            fileWriter = new FileWriter(fileB);
            int len;
            char[] cbuf = new char[10];
            while ((len = fileReader.read(cbuf)) != -1) {
                fileWriter.write(cbuf, 0, len);
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                if (fileReader != null)
                    fileReader.close();
                if (fileWriter != null)
                    fileWriter.close();
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }

    public static void main(String[] args) {
        new TestCharStream().copyFile("data.txt", "file.txt");
    }

    @Test
    public void testFileReadWite() {
        TestCharStream testIO = new TestCharStream();
        testIO.Write("""
                public void Read() {
                    try {
                        FileReader fileReader = new FileReader(file);
                        int read;
                        while ((read = fileReader.read()) != -1)
                            System.out.print((char)read);
                        fileReader.close();
                    } catch (IOException e) {
                        e.printStackTrace();
                    }
                }

                public void Write(String toBeWrite) {
                    try {
                        FileWriter fileWriter = new FileWriter(file);
                        fileWriter.append(toBeWrite);
                        fileWriter.close();
                    } catch (IOException e) {
                        e.printStackTrace();
                    }
                }
                """);
        testIO.Read();
    }
}
```
### Socket与网络编程
1.网络编程的两个主要问题 1)如何定位网络上的一台或多态主机,定位主机上的特定应用(IPhe端口) 2)找到主机后如何高效可靠的传输数据(TCP/IP)

本地回环地址: (hostAddress):127.0.0.1

主机名: (hostName):localhost

局域网地址: 192.168.~

端口号: 运行的程序(0-65535)

Sockst = ip + 端口号

TCP 可靠, 大量(三次握手, 四次挥手)

UDP 快速

**Url类**
```java
package lang.net;

import java.net.URL;

/**
 * url https://www.bilibili.com/video/BV1Kb411W75N?p=629
 * 
 * <协议>//:<主机名>:<端口号>/<文件名/>#<片段名?><参数列表>
 */

public class MyUrl {
    public static void main(String[] args) {
        try {
            URL url = new URL("https://www.bilibili.com/video/BV1Kb411W75N?p=629");
            System.out.println(url.getAuthority());// www.bilibili.com
            System.out.println(url.getDefaultPort());// 443
            System.out.println(url.getFile());// /video/BV1Kb411W75N?p=629
            System.out.println(url.getPath());// /video/BV1Kb411W75N
            System.out.println(url.getProtocol());// https
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```
**InetAddress类**
```java
        try {
            InetAddress Aliyun = InetAddress.getByName("39.99.54.127");
            InetAddress localHost = InetAddress.getLocalHost();
            InetAddress loopbackAddress = InetAddress.getLoopbackAddress();
            boolean reachable = localHost.isReachable(10);
            System.out.println(reachable);//true
            System.out.println(localHost);//DESKTOP-V9EL4A5/192.168.0.193
            System.out.println(Aliyun);///39.99.54.127
            String canonicalHostName = Aliyun.getCanonicalHostName();
            String hostName = Aliyun.getHostName();//39.99.54.127
            System.out.println(hostName);//39.99.54.127
            System.out.println(canonicalHostName);
            System.out.println(loopbackAddress);//localhost/127.0.0.1
        } catch (Exception e) {
            e.printStackTrace();
        }
```
**UDP网络编程**
```java
	/**
     * 接收端
     */
    @Test
    public void UDPReciver() {
        DatagramSocket server = null;
        try {
            server = new DatagramSocket(8900);
            byte[] buf = new byte[1024];// <=8K
            DatagramPacket datagramPacket = new DatagramPacket(buf, buf.length);
            System.out.println("waiting...");
            while (true) {
                server.receive(datagramPacket);
                String string = new String(datagramPacket.getData(), 0, datagramPacket.getLength());
                System.out.println(datagramPacket.getAddress() + ":" + datagramPacket.getPort() + "send: " + string);
            }
        } catch (SocketException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (server != null)
                server.close();
        }

    }
	/**
     * 发送端
     */
    @Test
    public void UDPSender() {
        DatagramSocket client = null;
        try {
            client = new DatagramSocket();
            String str = "hello Quanwei";
            DatagramPacket datagramPacket = new DatagramPacket(str.getBytes(), str.getBytes().length,
                    InetAddress.getLocalHost(), 8900);
            System.out.println("start send...");
            client.send(datagramPacket);
        } catch (SocketException | UnknownHostException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (client != null)
                client.close();
        }
    }
```
**TCP网络编程**
```java
package lang.net;

import java.io.ByteArrayOutputStream;
import java.io.File;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.net.InetAddress;
import java.net.ServerSocket;
import java.net.Socket;
import java.net.UnknownHostException;
import java.time.LocalDateTime;

public class TCP {

    public void clientResive() {
        Socket socket = null;
        InputStream inputStream = null;
        ByteArrayOutputStream byteArrayOutputStream = null;
        try {
            // get from server
            InetAddress ip = InetAddress.getByName("127.0.0.1");
            socket = new Socket(ip, 8899);
            inputStream = socket.getInputStream();
            byte[] buf = new byte[1024];
            int len;
            byteArrayOutputStream = new ByteArrayOutputStream();
            while ((len = inputStream.read(buf)) != -1) {
                byteArrayOutputStream.write(buf, 0, len);
            }

            System.out.println(LocalDateTime.now() + " get from server: " + byteArrayOutputStream);

        } catch (UnknownHostException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (byteArrayOutputStream != null)
                try {
                    byteArrayOutputStream.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            if (inputStream != null)
                try {
                    inputStream.close();
                } catch (IOException e1) {
                    e1.printStackTrace();
                }
            if (socket != null)
                try {
                    socket.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
        }
    }

    public void clientSend() {
        Socket socket = null;
        InetAddress ip;
        OutputStream outputStream = null;

        try {
            ip = InetAddress.getByName("127.0.0.1");
            socket = new Socket(ip, 8899);
            // send to server
            outputStream = socket.getOutputStream();
            outputStream.write("权威 Quanwei".getBytes());
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            if (outputStream != null)
                try {
                    outputStream.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            if (socket != null)
                try {
                    socket.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
        }
    }

    public void serverRevive() {
        ServerSocket server = null;
        Socket accept = null;
        InputStream inputStream = null;
        ByteArrayOutputStream byteArrayOutputStream = null;
        FileOutputStream fileOutputStream = null;
        try {
            server = new ServerSocket(8899);
            accept = server.accept();
            // client input
            inputStream = accept.getInputStream();
            byteArrayOutputStream = new ByteArrayOutputStream();
            byte[] buf = new byte[1024];
            int len;
            while ((len = inputStream.read(buf)) != -1) {
                byteArrayOutputStream.write(buf, 0, len);
            }
            // 控制台输出
            System.out.println(LocalDateTime.now() + " get from client: " + byteArrayOutputStream);

            // 保存到本地
            fileOutputStream = new FileOutputStream(new File("D:\\workspaceFolder\\CODE_JAVA\\lang\\net\\file"));
            fileOutputStream.write(byteArrayOutputStream.toByteArray());

        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (byteArrayOutputStream != null)
                try {
                    byteArrayOutputStream.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            if (inputStream != null)
                try {
                    inputStream.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            if (accept != null)
                try {
                    accept.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            if (server != null)
                try {
                    server.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
        }
    }

    public void serverSend() {
        ServerSocket server = null;
        Socket accept = null;
        OutputStream outputStream = null;
        try {
            server = new ServerSocket(8899);
            accept = server.accept();
            // server output
            outputStream = accept.getOutputStream();
            outputStream.write("接收成功".getBytes());
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            if (outputStream != null)
                try {
                    outputStream.close();
                } catch (IOException e1) {
                    e1.printStackTrace();
                }
            if (accept != null)
                try {
                    accept.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            if (server != null)
                try {
                    server.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
        }
    }

    public static void main(String[] args) {
        TCP tcp = new TCP();
        new Thread(() -> {
            tcp.serverSend(); // 先启动服务器
        }).start();
        // new Thread(() -> {
        //     tcp.clientResive();
        // }).start();
        new Thread(() -> {
            tcp.clientResive();
        }).start();
        // new Thread(() -> {
        //     tcp.clientSend();
        // }).start();
    }
}
```
## 6.2 NIO

## 6.3 AIO

# 七、多线程
	
	多线程是Java最基本的一种并发模型

一个线程不能独立的存在，它必须是进程的一部分。
进程：一个进程包括由操作系统分配的内存空间，包含一个或多个线程。

创建多线程
```java
package lang.thread;

import org.junit.jupiter.api.Test;

public class CreateThreadDemo {

    @Test
    public void testLambda() {
        new Thread(() -> {
            String name = Thread.currentThread().getName();
            System.out.println(name);// Quanwei's thread
        }, "Quanwei's thread").start();
    }

    @Test
    public void testExtends() {
        new Thread() {
            public void run() {
                String name = Thread.currentThread().getName();
                System.out.println("Extends" + name);// ExtendsThread-0 也可能不是0
            }
        }.start();
        ;
    }

    @Test
    public void testImplements() {
        Runnable runnable = new Runnable() {
            @Override
            public void run() {
                String name = Thread.currentThread().getName();
                System.out.println(name);// Implements
            }
        };
        new Thread(runnable,"Implements").start();
    }

    public static void main(String[] args) {
        new CreateThreadDemo().testLambda();// main
        System.out.println(Thread.currentThread().getName());
    }

}
```
多线程通信
同步代码块

synchronized(同步监视器){//同步监视器可用: 类.class(保证唯一)
//要同步的代码
}

不需要synchronized的操作 JVM规范定义了几种原子操作：
1. 基本类型（long和double除外）赋值，例如：int n = m； 引用类型赋值，例如：List<String> list = anotherList。
2. long和double是64位数据，JVM没有明确规定64位赋值操作是不是一个原子操作，不过在x64平台的JVM是把long和double的赋值作为原子操作实现的
3. 单条原子操作不需要线程同步,多条需要

```java
public class Ticket {
    public static void main(String[] args) {
        var w = new Windows1();
        var w1 = new Thread(w, "w1");
        var w2 = new Thread(w, "w2");
        var w3 = new Thread(w, "w3");
        w2.setPriority(Thread.MAX_PRIORITY);
        w1.start();
        w2.start();
        w3.start();
    }
}

class Windows1 implements Runnable {
    private int tickets = 50;
    Object obj = new Object();

    @Override
    public void run() {
        while (true) {
            synchronized (Windows1.class) {
                if (tickets > 0) {
                    try {
                        Thread.sleep(100);
                    } catch (Exception e) {
                    }
                    System.out.println(Thread.currentThread().getName() + " : " + tickets);
                    --tickets;
                } else
                    break;
            }
        }
    }

}

```
# 八、JDBC
什么是JDBC?

	DBC是Java DataBase Connectivity的缩写，它是Java程序访问数据库的标准接口。

使用Java程序访问数据库时，Java代码并不是直接通过TCP连接去访问数据库，而是通过JDBC接口来访问，而JDBC接口则通过JDBC驱动来实现真正对数据库的访问。

我们在Java代码中如果要访问MySQL，那么必须编写代码操作JDBC接口。注意到JDBC接口是Java标准库自带的，所以可以直接编译。而具体的JDBC驱动是由数据库厂商提供的，。因此，访问某个具体的数据库，我们只需要引入该厂商提供的JDBC驱动，就可以通过JDBC接口来访问，这样保证了Java程序编写的是一套数据库访问代码，却可以访问各种不同的数据库，因为他们都提供了标准的JDBC驱动

如何使用? (以连接PostgreSQL为例)
```java
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.Properties;
import org.postgresql.Driver;

public class Postgresql {
    private static String url = "jdbc:postgresql://localhost:5432/quanwei";
    private static String user = "postgres";
    private static String password = "password";
    private static String driver = "org.postgresql.Driver";

    public static void main(String[] args) throws SQLException {
        // 1. 创建驱动程序类对象
        Driver driver = new org.postgresql.Driver();

        // 2. 设置用户名和密码
        Properties prop = new Properties();
        prop.setProperty("user", user);
        prop.setProperty("password", password);

        // 3. 连接数据库，返回连接对象
        Connection conn = null;
        try {
            conn = driver.connect(url, prop);
        } catch (SQLException e) {
            e.printStackTrace();
        }
        Statement s = conn.createStatement();
        // 测试
        ResultSet rs = s.executeQuery("select * from login");
        while(rs.next()){
            String string = rs.getString(2);
            System.out.println(string);
        }
    }
}

```
# 九、GUI
## 9.1 AWT
即抽象窗口工具包
```java
package lang.gui;

import java.awt.*;
import java.awt.event.*;

public class HelloAWT {
    public static void main(String[] args) {
        // 1.创建窗体
        Frame frame = new Frame("AWT");
        // 2.创建标签组件
        Label label = new Label("Hello Quanwei");
        // 3.设置相关属性
        label.setBackground(Color.GREEN);
        label.setSize(20, 10);
        // 4.将组件添加到窗体上
        frame.add(label);
        // 5.设置窗体关闭事件
        frame.addWindowListener(new WindowAdapter() {
            @Override
            public void windowClosing(WindowEvent e) {
                System.exit(0);
            }
        });
        // 6.设置窗体大小
        frame.setSize(500, 200);
        // 7.显示窗体
        frame.setVisible(true);
    }
}
```
## 9.2 Swing
```java
package lang.gui;

import java.awt.*;
import java.awt.event.*;
import java.util.HashMap;

import javax.swing.*;
import javax.swing.event.*;

public class Main {

    public void window1() {
        // 1.创建窗体
        JFrame frame = new JFrame("Swing");
        // 2.设置关闭事件
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        // 3.设置大小位置
        frame.setSize(500, 500);
        frame.setLocation(500, 200);

        // 4.设置事件监听处理
        frame.addMouseListener(new MouseInputAdapter() {
            @Override
            public void mouseClicked(MouseEvent e) {
                int x = e.getX();
                int y = e.getY();
                frame.setTitle("点击坐标为(" + x + ", " + y + ")");
            }
        });
        // 5.显示窗体
        frame.setVisible(true);
    }
}
```
# 总结
比我想象的要多得多😂😂😂🤣🤣🤣
