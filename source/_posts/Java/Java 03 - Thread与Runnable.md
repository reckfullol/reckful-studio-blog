---
title: Java 03 - Thread与Runnable
date: 2018-07-12 14:21:53
categories: Java
---
# Thread与Runnable

<!--more-->

在Java创建一个线程的时候, 通常是有两种方法, 一种是重写run()方法, 调用对象实例的start()方法; 一种是实现Runnable接口, 重写run()方法, 将对象实例作为Thread类初始化方法的实参, 并调用新生成的Thread类实例的start()方法.

## Thread

older syntax:

```java
public class MyThread extends Thread {
    public void run() {
        // your code here
    }
}

MyThread myThread = new MyThread();
myTread.start();
```

或者:

```java
Thread thread = new Thread() {
    public void run() {
        // your code here
    }
}

thread.start();
```

lambda:

```java
new Thread(() -> /* your code here*/ ).start();
```

## Runnable

older syntax:

```java
// pre java 8 lambdas
Thread t = new Thread(new Runnable() {
    public void run() {
        // your code here ...
    }
});

t.start();
```

lambda:

```java
Runnable runnable = () -> { 
    // your code here ...
};
Thread t = new Thread(runnable);
t.start();
```

## 对比

实现Runnable接口相较于继承Thread的优势:

1. 适合多个相同的程序代码的线程去处理一个资源, 在实现Runnable接口的实例中, 各个线程共享该实例的数据域, 但是可能需要进行同步约束.

1. 可以避免Java中的单继承限制.