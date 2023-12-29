---
title: Java 语言特性
date: 2023-06-02 19:57:56
tags: Java
---

## 基本特性

Java语言的特性包括：

1. 简单易学：Java语言的语法与C++类似，但是去掉了一些复杂的特性，使得Java更加容易学习和使用。

2. 面向对象：Java是一种纯面向对象的编程语言，所有的数据类型都是对象，所有的操作都是方法调用。

3. 平台无关性：Java程序可以在不同的平台上运行，只需要安装相应的JVM即可。

4. 安全性：Java提供了严格的安全机制，防止恶意代码对系统造成破坏。

5. 自动内存管理：Java自动进行垃圾回收，程序员不需要手动管理内存。

6. 多线程支持：Java提供了多线程支持，可以方便地实现并发编程。

7. 异常处理：Java提供了异常处理机制，可以有效地处理程序中出现的错误。

8. 开放性：Java是一种开放的编程语言，有大量的开源库和框架可供使用。

9. 高性能：Java虚拟机（JVM）可以将字节码转换为本地机器码执行，具有较高的性能。

10. 动态性：Java支持动态加载和卸载类，可以在运行时动态地修改程序行为。


## 版本的新特性

Java历代版本的新特性如下：

## Java 1.0：

Java语言首次发布，包括基本的面向对象特性和网络编程API。

## Java 1.1：

增加了内部类、反射、JAR文件格式等特性，并引入了AWT（Abstract Window Toolkit）图形用户界面库。

## Java 1.2：

引入了Java虚拟机的HotSpot技术，提高了性能；增加了Swing图形用户界面库、集合框架、JavaBeans组件 

## Java 1.3：

增加了JNDI（Java Naming and Directory Interface）   
Java Sound API   
JPDA（Java Platform Debugger Architecture）调试器 

## Java 1.4：

增加了NIO（New Input/Output）库   
正则表达式支持   
XML解析器 

## Java 5.0（Java SE 5）：

引入了泛型   
自动装箱/拆箱   
枚举类型   
注解   
可变参数 

## Java 6（Java SE 6）：

增加了JDBC 4.0   
JAX-WS 2.0   
Java Compiler API等特性，并优化了JVM性能。

## Java 7（Java SE 7）：

增加了switch语句支持字符串   
二进制字面量   
try-with-resources语句   
Diamond操作符 

## Java 8（Java SE 8）：

引入了Lambda表达式   
Stream API   
Date/Time API   
默认方法 

## Java 9（Java SE 9）：

引入了模块化系统   

	JDK9将JDK分成一组模块，可以在编译时或运行时进行组合。这样可以减少内存开销，只需必要的模块，并非全部模块，可以简化各种类库和大型应用的开发和维护。比如在使用java开发的时候通常用不到图形化界面的库，分模块之后，在java的base模块中没有这些图形化相关的内容，达到了一个瘦身的效果

接口私有方法    

	```java
	public interface MyInterface {
		//定义私有方法
		private void m1() {
		System.out.println("123");
	}

	//default中调用
	default void m2() {
		
	}
	}
	```
JShell交互式命令行工具  

不能使用下划线命名变量

String字符串的变化

    jdk9中将String内部的char数组改成了byte数组，这样就节省了一半的内存占用

HTTP/2客户端 

改进的try with resource

## Java 10（Java SE 10）：

增加了局部变量类型推断   

    类型都可以使用var来代替，JVM会自动推断该变量是什么类型的

G1垃圾回收器改进


## Java 11（Java SE 11）：
java直接运行

	在以前的版本中，我们在命令提示下，需要先编译，生成class文件之后再运行
增加了HTTP客户端API   
ZGC垃圾回收器   
Epsilon垃圾回收器  
String新增方法

	isBlank方法，判断字符串长度是否为0，或者是否是空格，制表符等其他空白字符  
	strip方法，可以去除首尾空格，与之前的trim的区别是还可以去除unicode编码的空白字符  
	repeat方法，字符串重复的次数  

lambda表达式中的变量类型推断

	允许在lambda表达式的参数中使用var修饰

	MyInterface mi = (var a,var b)->{
       System.out.println(a);
       System.out.println(b);
    };

## Java 12（Java SE 12）：
增加了Switch表达式   
```java
int month = 3;
switch (month) {
	case 3,4,5 -> System.out.println("spring");
	case 6,7,8 -> System.out.println("summer");
	case 9,10,11 -> System.out.println("autumn");
	case 12, 1,2 -> System.out.println("winter");
	default -> System.out.println("wrong");
}
```
JVM常量API 

## Java 13（Java SE 13）：

Switch表达式增强 
```java
int month = 3;
String result = switch (month) {
	case 3,4,5 -> "spring";
	case 6,7,8 -> "summer";
	case 9,10,11 -> "autumn";
	case 12, 1,2 -> "winter";
	default -> "wrong";
};
```
增加了文本块  
```java
String s = """
            Hello
            World
            Learn
            Java
           """;
  System.out.println(s);
```

## Java 14（Java SE 14）：

增加了Records   

```java
public record User(String name,Integer age){}
```
	记录类型有自动生成的成员，包括：

    状态描述中的每个组件都有对应的private final字段。状态描述中的每个组件都有对应的public访问方法。方法的名称与组件名称相同。一个包含全部组件的公开构造器，用来初始化对应组件。实现了equals()和hashCode()方法。equals()要求全部组件都必须相等。实现了toString()，输出全部组件的信息。

Pattern Matching for instanceof 

```java
if(obj instanceof Integer){
		Integer i = (Integer)obj;
		int result = i + 10;
		System.out.println(i);
	}
```

友好的空指针（NullPointerException）提示

	错误信息就可以明确的指出那个对象为null了。此外，还可以使用下面参数来查看:

	java -XX:+ShowCodeDetailsInExceptionMessages <className>

Sealed Classes密封类和接口

	作用是限制一个类可以由哪些子类继承或者实现。

    如果指定模块的话，sealed class和其子类必须在同一个模块下。如果没有指定模块，则需要在同一个包下。sealed class指定的子类必须直接继承该sealed class。sealed class的子类要用final修饰。sealed class的子类如果不想用final修饰的话，可以将子类声明为sealed class

CharSequence新增的方法

	该接口中新增了default方法isEmpty()，作用是判断CharSequence是否为空。

TreeMap新增方法

    putIfAbsent
	computeIfAbsent
	computeIfPresent
	computemerge
## Java 15（Java SE 15）：

增加了Sealed Classes   

Text Blocks增强 

## Java 16（Java SE 16）：

增加了Records增强   

Pattern Matching for switch

新增日时段

	在DateTimeFormatter.ofPattern传入B可以获取现在时间对应的日时段，上午，下午等
```java
	System.out.println(DateTimeFormatter.ofPattern("B").format(LocalDateTime.now()));
```
InvocationHandler新增方法

	在该接口中添加了下面方法
```java
public static Object invokeDefault(Object proxy, Method method, Object... args)
```

## Java 17 (Java SE 17) :

instanceof模式匹配

```JAVA
switch(a){
	//如果a是Rabbit类型，则在强转之后赋值给r，然后再调用其特有的run方法
	case Rabbit r -> r.run();
	//如果a是Bird类型，则在强转之后赋值给b，然后调用其特有的fly方法
	case Bird b -> b.fly();
	//支持null的判断
	case null -> System.out.println("null");
	default -> System.out.println("no animal");
}
```

伪随机数的变化

增加了伪随机数相关的类和接口来让开发者使用stream流进行操作

    RandomGenerator
	RandomGeneratorFactory

去除了AOT和JIT

	AOT（Ahead-of-Time）是java9中新增的功能，可以先将应用中中的字节码编译成机器码。

	Graal编译器作为使用java开发的JIT（just-in-time ）即时编译器

## Java 18 (Java SE 18) :

	从jdk18开始，默认使用UTF-8字符编码。我们可以通过如下参数修改其他字符编码：

	-Dfile.encoding=UTF-8 

简单的web服务器

	可以通过jwebserver命令启动jdk18中提供的静态web服务器，可以利用该工具查看一些原型，做简单的测试。在命令提示符中输入jwebserver命令后会启动，然后在浏览器中输入:http://127.0.0.1:8000/ 即可看到当前命令提示符路径下的文件了。
	将被移除的方法

在jdk18中标记了Object中的finalize方法，Thread中的stop方法将在未来被移除。

@snippet注解
	以前在文档注释中编写代码时需要添加code标签，使用较为不便，通过@snippet注解可以更方便的将文档注释中的代码展示在api文档中。

## Java 19 (Java SE 19) :

JEP 405 - Record 模式
```JAVA
record Point(int x, int y) {}
static void printSum(Object o) {
	if (o instanceof Point(int x, int y)) {
		System.out.println(x + y);
	}
}

public static void main(String[] args) {
	printSum(new Point(3, 6));
}
```

JEP 427 - switch 模式匹配

	1.增强类型检查，case 表达式支持多种类型	
	2.switch 表达式和语句分支全覆盖检测
	3.扩展了模式变量声明范围
	4.优化 null 处理，可以声明一个 null case

JEP 422 - Linux/RISC-V 移植

JEP 424 - 外部函数和内存 API 

	易用性：通过卓越的纯 Java 开发模型代替 JNI
	高性能：提供能与当前 JNI 和 sun.misc.Unsafe 相当甚至更优越的性能
	通用性：提供支持不同种类的外部内存（如本地内存、持久化内存和托管堆内存）的 API，并随着时间推移支持其他操作系统甚至其他语言编写的外部函数
	安全性：允许程序对外部内存执行不安全的操作，但默认警告用户此类操作

	分配外部内存：MemorySegment、MemoryAddress和SegmentAllocator
	操作和访问结构化的外部内存：MemoryLayout和VarHandle
	控制外部内存(分配和回收) ：MemorySession
	调用外部函数：Linker、FunctionDescriptor和SymbolLookup

JEP 426 - 向量 API

JEP 425 - 虚拟线程

JEP 428 - 结构化并发

## Java 20 (Java SE 20) :

JEP 429：作用域值（Scoped Values，孵化阶段）

JEP 432：记录模式（Record Patterns，第二轮预览）

JEP 433: switch的模式匹配（Pattern Matching for switch，第四轮预览）

JEP 434：外部函数与内存API（Foreign Function & Memory API，第二轮预览）

JEP 436：虚拟线程（Virtual Threads，第二轮预览）

JEP 437：结构化并发（Structured Concurrency，第二轮孵化）

JEP 438：Vector API（第五轮孵化）