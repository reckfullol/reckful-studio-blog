---
title: Java 04 - synchronized
date: 2018-07-12 23:10:56
categories: Java
---
# synchronized

<!--more-->

## 几个概念

1. 临界资源: 每次仅允许一个进程访问的资源.

2. 临界区: 对临界资源访问的区域.

3. 互斥: 多个进程在同一时刻只有一个进程能进入临界区.

## synchronized

首先我们需要知道互斥锁的概念, 是能够达到互斥访问目的的锁. 那么如果对临界资源加上互斥锁, 当一个线程在访问该临界资源的时候, 其他线程只能等待.

在Java中, 每个对象都拥有锁标记(monitor), 也称为监视器. 多线程同时访问某一对象的时候, 只有获取了该对象所的线程才能访问.

在Java中, 可以使用synchronized关键字来修饰一个方法或者一个代码块, 当某个线程调用被修饰的方法或者代码块时, 这个线程便获得了这个对象的锁, 其余线程就无法访问. 只有等待这个线程退出临界区, 才会释放该对象的锁, 其余线程才能访问.

```java
import java.util.ArrayList;

class MyArrayList {
    private ArrayList<Integer> arrayList = new ArrayList<>();
    synchronized void insert(Thread thread) {
        for(int i = 0; i < 10; i++) {
            System.out.println("thread " + thread.getName() + " : insert " + i);
            arrayList.add(i);
        }
    }
}

public class Test {
    public static void main(String[] args) {
        final var myArrayList = new MyArrayList();
        new Thread(() -> myArrayList.insert(Thread.currentThread())).start();
        new Thread(() -> myArrayList.insert(Thread.currentThread())).start();
    }
}
```

## 几点注意

1. 当一个线程在访问某一个对象的synchronized方法时, 其他线程不能访问该对象的其他synchronized方法. 因为一个对象只有一个锁. 当该线程获得对象的锁后, 其他线程便无法获得该对象的锁, 自然不能访问其synchronized方法.

1. 当一个线程在访问某一个对象的synchronized方法是, 其他线程可以访问非synchronized方法, 因为这并不需要获得对象锁.

1. 如果一个线程访问某一个对象的非static synchronized方法, 另一个线程访问同一对象的static synchronized方法, 并不会发生互斥. 因为一个占用的是对象锁, 一个占用的是类锁.

1. 对于synchronized方法或者代码块, 当出现异常的时候, JVM会自动释放线程占用的锁, 因此不会由于异常导致死锁的现象.

## block 与 wait

如果我们对含有synchronized方法或者代码段的class进行反编译, 就可以从字节码中看到monitorenter和monitorexit两条指令. 前者会让对象锁计数加一, 后者减一. 类似于操作系统中的PV原语.

值得注意的是线程的block和wait状态是两个完全不同的状态.

1. block状态是指线程想访问对象的synchronized方法或者代码块, 但是对象的锁已经被其他线程持有, 那么该线程变成block态, 进入阻塞队列, 等待对象锁的释放. 

1. wait状态是已经持有对象锁的线程主动调用wait()方法, 把自身挂起进入wait set中, 同时释放对象锁. 当发生下列任意情况, 线程会退出wait set:
    - 有其他线程调用notify方法唤醒线程.

    - 有其他线程调用notify all方法唤醒线程.
     
    - 有其他线程以interrupt方法唤醒线程, 当前线程捕捉到InterruptException.
     
    - wait方法到期(timeout).

由此可见, block态和wait态最大的区别在于前者是有JVM调度, 而后者是在代码中主动调用并调度的.

值得注意的是:

1. wait(), notify(), notifyAll()都是java.lang的Object类的方法, 而不是Thread类的方法. 因为这三个方法都是对于实例的wait set进行操作的, 而wait set是作用于所有Object的.

2. 如果没有拿到对象锁的线程调用上面三种方法, 会触发IllegalMonitorStateException异常.