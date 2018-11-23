---
title: Java 05 - volatile
date: 2018-07-14 04:24:22
categories: Java
---
# volatile

<!--more-->

## 几个概念

1. 原子性

    原子性是指, 对于一个操作或者多个操作, 要么全部执行并不会被打断, 要么都不执行.

    在Java中, 基本类型, 引用类型的赋值和引用是原子操作. 在早期的JVM实现中, double和long是可以被分割的, 但在实际开发中, 目前各种平台下的商用虚拟机几乎都会选择把64位数据的读写操作作为原子操作来对待. 因此在使用long和double变量时一般不需要专门声明volatile.

2. 可见性

    可见性是指, 当多个线程访问同一个变量时, 一个线程修改了这个变量的值, 其他线程能够立即看到修改后的值.

    Java提供了关键字volatile来保证可见性, 即被volatile修饰的变量在修改后会把值更新到主存上, 当其他线程需要读取的时候, 会从内存中读取新值.

    普通的共享变量不能保证可见性, 因此写入主存的时间是不确定的.

    同时synchronized和lock也可以保证共享变量的可见性.

3. 有序性

    有序性是指, 程序的执行顺序是按照代码的先后顺序执行的.

    在JVM中允许编译器和处理器对指令进行重排序, 可以提高执行效率. 值得注意的是, 重排序不会影响单进程程序的执行结果, 但是可能会影响多进程程序的执行结果.

    在JMM中有happen-before原则, 如果两个操作的执行顺序无法满足happen-before, 那么就不能其有序性, JVM就可以对其进行重排序.

## volatile作用

1. 保证了不同线程对共享变量操作的可见性, 即一个线程修改了某个共享变量的值, 那么这个新值对其他线程是可见的.

    ```java
    // thread 01
    boolean stop = false;
    while(!stop) {
        doSomething()
    }

    // thread 02
    stop = true;
    ```

    上面的代码, 当线程2修改了stop的值, 但是还没来得及写入主存, 那么线程1就会一直在循环中调用doSomething方法.

    那么当用volatile修饰stop变量后:

    1. 线程2修改stop后值会强制写入主存.

    2. 线程2修改后, 线程1工作内存缓存变量stop无效, 线程1会主动到主存中获取最新值.

1. 禁止进行指令重排序
    1. 当程序执行到volatile变量的读或写操作时, 在前面的操作的更改肯定是已经进行, 并且结果对后面的操作可见; 在其后面的操作肯定还没有进行.

    1. 在进行指令优化时, 不能将对volatile变量访问的语句放在后面执行, 也不能把volatile后面的语句放到前面执行.

    举个例子:

    ```java
    // thread 01
    context = loadContext();
    inited = true;

    // thread 02
    while(!inited) {
        sleep();
    }
    doSomething();
    ```

    由于线程1中的语句1和语句2没有数据依赖关系, 因此有可能会被重排, 那么在context没有读取完毕的情况下, 线程2就有可能做下一步的操作.

    由此可见, 指令重排序不会影响单个线程的执行, 但是会影响到线程并发性的正确性.

    那么我们用volatile修饰inited后就不会出现这个问题了. 因为这样在编译期间, 会在指令序列中插入内存屏障来禁止特定类型的处理器重排序.

## volatile实现

> 观察加入volatile关键字和没有加入volatile关键字时所生成的汇编代码发现，加入volatile关键字时，会多出一个lock前缀指令.
> 
>  <深入理解Java虚拟机>

LOCK指令前缀会设置处理器的LOCK#信号, 这个信号会使总线锁定, 阻止其他处理器接管总线的访问, 使得指令的执行变成原子操作, 完成处理器对共享内存的独享使用.

LOCK指令前缀实际上相当于一个内存屏障, 会提供3个功能:

1. 确保重排序时不会把后面的指令放到内存屏障之前的位置, 也不会把前面的指令放到内存屏障之后的位置, 即保证到内存屏障这个指令时, 前面的操作已经全部完成.

1. 强制将对缓存的修改立即写到主存.

1. 如果是写操作, 那么其他CPU的缓存行无效.

## volatile应用场景

应用前提:

1. 对变量的写操作不依赖于当前值.

2. 变量没有包含在具有其他变量的不变式中.

应用场景:

1. 状态标记量

    ```java
        // thread 01
        context = loadContext();
        volatile boolean inited = true;

        // thread 02
        while(!inited) {
            sleep();
        }
        doSomething();
    ```

2. double check

    ```java
    class Singleton {
        private volatile static Singleton instance = null;

        private Singleton() {

        }

        public static Singleton getInstance() {
            if(instance == null) {
                synchronized(Singleton.class) {
                    if(instance == null) {
                        instance = new Singleton();
                    }
                }
            }
            return instance;
        }
    }
    ```

    这样可以减少每次获取单例是都要获得类锁的开销, 但是需要对引用用volatile修饰, 因为在Java中不同步的情况下引用不是线程安全的, 所以如果不用volatile修饰, double check是无效的.

    其实这里更推荐Initialization on Demand Holder的方法来实现单例模式:

    ```java
    class Singleton {
        static class SingletonHolder {
            static Singleton instance = new Singleton();
        }
        public static Singleton getSingleton() {
            return SingletonHolder.instance;
        }
    }
    ```

    我们知道, 在Java中初始化一个类, 包括执行这个类的静态初始化和在这个类中的静态字段. 当发生下列任意情况后, 类会被初始化:

    - 类的一个实例被创建.

    - 类中的一个静态方法被调用.

    - 类中声明的一个静态字段被赋值.

    - 类中一个静态字段被使用, 同时不是常量.

    - 类是一个顶级类, 同时内部有一个断言语句被执行.

    在Java中每一个类都有一个唯一的初始化锁与之对应, JVM在类的初始化期间会获得这个锁, 从而实现线程安全的情况下完成对类的初始化工作.

    综上我们可以看到基于类初始化的方案更加优雅, 但是volatile的double check还有一个优势, 就是可以完成对实例字段的延迟初始化.