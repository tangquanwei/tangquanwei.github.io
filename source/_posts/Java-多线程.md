---
title: Java 多线程
date: 2023-04-15 09:07:35
tags: Java
---

# Java 多线程

## synchronized

对于非静态成员：锁加在 类.class 对象上
对于静态成员：锁加在 this 对象上

锁是什么？
  锁是一个”对象“：
  1. 对象中有一个标志位，记录自己有没有被某个线程占用。
  2. 如果被占用记录线程的id,知道是那个线程。
  3. 维护一个 thread id list,记录其他所有阻塞的、等待拿锁的线程。
  4. 锁是“对象”，共享资源也是”对象“，二者可合二为一。

### wait notify

wait notify和synchronized 作用的对象是同一个

wait 
  1. 释放锁
  2. 阻塞，等待被其他对象notify
  3. 重新拿锁

## volatile

使用内存屏障实现

内存可见性
  多核逻辑CUP缓存和内存
  

指令重排序
  1. 没有依赖关系的指令可能重排序

as-if-serial 语义
  和单线程执行结果相同

happen-before
  若 A happen-before B,则A的执行结果对B可见。

## Compare And Set (CAS)

悲观锁：认为并发冲突的概率很大，所以读操作之前就上锁。

乐观锁：认为并发冲突的概率很小，所以读之前不加锁，等写的时候判断是否被其他线程修改（1）
如果修改了就把数据重新读出来，重复该过程如果没修改，就写回去（2）。
（1）（2）合并为一个原子操作。

拿不到锁时的策略：
  1. 自旋：不放弃CUP,空转，不断重试。
  2. 阻塞：放弃CUP,进入阻塞状态，等待被唤醒，再被操作系统调度。
  3. 先自旋再阻塞

## 原子类

AtomicInteger

AtomicLong

AtomicReference

AtomicBoolean

```java
if(f==false){
  f=true
}
```

### 解决ABA问题

A->B->A 从结果不能看出是否修改

解决方法：不仅比较“值”还比较“版本号”


AtomicStampedReference 使用累加量
```java
Integer i = 100;
int is = 1;
var ai = new AtomicStampedReference<>(i, is);
System.out.println(ai.compareAndSet(i, ai.getReference(), is, ai.getStamp()));
ai.set(i + 1, is + 1);
System.out.println(ai.compareAndSet(i, ai.getReference(), is, ai.getStamp()));
```

AtomicMarkableReference 使用bool
```java
Integer i = 1;
var ai = new AtomicMarkableReference<>(i, false);
System.out.println(ai.compareAndSet(i,
        ai.getReference(),
        false,
        ai.isMarked()));
```
### 将已有类成员变量改为原子类型
