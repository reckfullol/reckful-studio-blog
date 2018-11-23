---
title: Java 06 - interrupt
date: 2018-07-20 10:54:31
categories: Java
---
# interrupt

<!--more-->

## 原理

interrupt是Thread中的一个方法, 其本质是将线程中的中断标志设置为true, 而不是直接中断. 设置后, 根据线程的状态而有不同的后续操作. 如果, 线程的当前状态处于非阻塞状态, 那么仅仅是线程的中断标志被改为了true, 一旦线程调用了wait, join, sleep方法中的一种, 立马抛出InterruptedException, 并将中断标志重置, 重新设置为false. 如果线程当前状态处于阻塞状态, 那么会有三种情况之一:

- 如果是wait, join, sleep三个方法引起的阻塞, 那么线程的中断标志会重置为false, 并且抛出一个InterruptedException.

- 如果是java.nio.channels.InterruptibleChannel进行的IO阻塞, 会抛出ClosedByInterruptedException.

- 如果是java.nio.channels.Selectors引起的阻塞, 则立即返回, 不会抛出异常.

## 循环体中catch

对InterruptedException的捕获一般放在while循环体的外面, 这样在产生异常的时候就退出了while循环, 否则InterruptedException在while之内, 就需要添加额外的退出处理.

```java
@Override
public void run() {
    while(true) {
        try {
            // todo
        } catch(InterruptedException) {
            // todo
            break;
        }
    }
}
```

## isInterrupted()

```java
@Override
public void run() {
    while(!isInterrupted()) {
        // todo
    }
}
```

isInterrupted()可以返回中断标志位的值, 表示运行中的线程是否被Thread.Interrupt()调用.

## interrputed()

Interrupted()和isInterrupted()都可以返回中断标志位的值, 不同的是, 前者还会将中断标志位重置为false.

## 中断状态与InterruptedException

调用Interrupt方法后, 可以中断掉线程. 这里的中断是指: 

1. 线程变成"中断状态".
2. 线程catch到InterruptedException后的逻辑操作.

因为我们知道, 如果调用了interrput方法, 但是线程的状态并不是在join, wait, sleep的话, 并不会抛出InterruptedException, 而是将interrupt标志位设为true, 这就是线程的"中断状态".

1. 中断状态 -> InterruptedException异常的转换:

    如果线程是中断状态, 那么抛出InterruptedException异常:

    ```java
    if(Thread.interrupted()) {
        throw new InterruptedException();
    }
    ```

    这样可以提高线程对于interrupt()的响应性. 注意Thread.interrupted()会抹掉标志位的值, 使其变成false. 如果不想对标志位改动, 可以调用Thread.currentThread().isInterrupted()方法.

2. InterruptedException异常 -> 中断状态的转换:

    ```java
    try {
        Thread.sleep(1000);
    } catch(InterruptedException e) {
        Thread.currentThread().interrput();
    }
    ```

3. InterruptedException异常 -> InterruptedException异常:

    ```java
    InterruptedException savedInterruption = null;
    
    try {
        Thread.sleep(1000);
    } catch (InterrputionException e) {
        savedException = e;
    }

    if(savedException != null) {
        throw savedException;
    }
    ```

    延迟抛出异常.


## 注意

处于wait态中的线程在被interrupt后, 会跳出wait set, 并在下次获得Object锁的时候, 才会抛出InterruptedException.