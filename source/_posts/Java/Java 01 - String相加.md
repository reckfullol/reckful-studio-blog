---
title: Java 01 - String相加
date: 2018-07-03 17:36:50
categories: Java
---
# String相加

<!--more-->

```java
public class StringDemo{
  private static final String MESSAGE="taobao";
  public static void main(String [] args) {
    String a = "tao"+"bao";
    String b = "tao";
    String c = "bao";
    System.out.println(a == MESSAGE);
    System.out.println((b + c) == MESSAGE);
  }
}
```

## Java内存使用

首先我们要知道, 在Java中的变量和基本类型的值是放在栈内存, 而new出来的对象存放在内存中, 指向对象的引用还是存放在栈内存的.

栈内存的一个特点是数据共享, 也就是说如果定义了

```java
int i = 1;
int j = 1;
```

那么i和j都会指向1这个常量地址, 如果i++, 那么常量值不会改变, i会去寻找栈中的新常量地址, 如果有就指向他, 如果没有就在栈中添加一个并指向他. 注意的是==操作是比较的内存地址是否相同, 那么当我们比较i == j的时候, 其实是比较所指向的常量地址. 所以

```java
String a = "abc";
String b = "abc";
System.out.println(a == b);
```

的输出也应该是true, 因为他们所指向的是栈中的同一常量"abc".

然而在堆内存中是没有数据共享的特点的. 所以

```java
String a = new String("abc");
String b = new String("abc");
System.out.println(a == b);
```

的输出应该是false, 因为他们在堆内存中是各自申请了一块空间. 如果我们是想比较他们的值是否相同, 可以调用equals()方法.

## 问题解析

那么回到最开始的问题, 这段代码输出的结果是什么. 事实上, 这段代码的输出分别是true和false.

```java
String a = "abc" + "def";
String a = "abcdef";
```

这两行代码编译后的字节码是一样的, 因为在编译期间, 编译器会会对字符串常量相加进行合并. 那么根据栈内存的数据共享, a和MESSAGE是指向栈中同一块常量空间.

第二段的(b+c)编译器没有办法对其在编译期进行优化, 因为只有到了运行期才知道b和c的值. 在Java中对String的相加是通过StringBuffer实现的, 先构造一个StringBuffer对象来存放"tao", 然后调用append()方法追加"bao", 然后将值为"taobao"的StringBuffer转化成String. 值得注意的是, 这个过程StringBuffer和String都是存放在堆内存中的. 因此(b+c)得到的对象和MESSAGE所指向的内存地址也不一样, 输出的结果就为false.

如果我们想让(b+c) == MESSAGE的输出结果为true, 那么有两种方法可以实现:

1. 调用intern()方法

    ```java
    System.out.println((b + c).intern() == MESSAGE);
    ```

    intern()方法会先检查栈内存中的String内存池中是否存在值相同的字符串常量, 如果存在, 那么返回该内存地址的引用. 因此当调用(b+c).intern()的时候就会返回和MESSGAE相同地址的引用, 输出结果也因此是true.

1. 用final修饰变量

    final方法修饰变量, 类似于c中的const, 可以认为变量变成了常量. 因此在编译期就会对(b+c)进行优化, 使得(b+c)返回的是"taobao"的地址, 输出的结果就为true.

## 注意

在JDK1.5以后, 如果经过逃逸分析发现代码中不存在线程安全问题, 那么就会将同步锁消除, 即使用StringBuilder这一非线程安全的容器完成String相加.