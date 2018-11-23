---
title: Java 12 - Lock
date: 2018-07-27 02:30:49
categories: Java
---
# Lock

<!--more-->

## synchronized缺陷

1. 当线程等待IO或者调用sleep()之类的方法, 导致被阻塞, 同时又没有释放掉锁, 那么其他线程就会一直在等待锁的释放. 那么如果线程可以定时放弃等待或者响应中断, 就可以提高执行效率. 这可以通过Lock实现.

2. 当有多个线程读写文件时, 如果用synchronized来实现同步, 只能由一个线程占有锁, 效率低.

## Lock的方法

Lock不是Java的内置, 而是一个类, 并通过这个类实现同步访问. 要注意的是, synchronized不需要手动释放锁, 当synchronized段执行完, 系统会自动让线程释放锁的占用. 而Lock需要手动释放锁, 如果没有主动释放, 有可能出现死锁的现象.

Lock是一个接口, 下面有lock(), tryLock(), tryLock(long time, TimeUnit unit), lockInterruptibly()是用来获取锁的, unLock()是用来释放锁的.

### 获得锁的方法

1. lock()

    lock()是最常用的方法, 用来获得锁. 如果锁已经被其他线程获取, 则进行等待.

    注意采用Lock, 必须主动去释放锁, 并且必须在try-catch块中进行, 将释放的过程放在finally块中, 保证锁一定被释放.

    ```java
    Lock lock = new Lock();
    lock.lock();
    try {
        // do
    } catch(Exception e) {

    } finally {
        lock.unlock();
    }
    ```

1. tryLock()

    tryLock()有返回值, 标志是否获得了锁. tryLock(long time, TimeUnit unit)则是如果拿不到锁会等待一定时间. 如果拿到了就返回true, 如果在时限内拿不到锁, 就返回false.

1. lockInterruptibly()

    lockInterruptibly()方法获得锁时, 如果线程在等待锁, 那么这个线程是可以响应中断的. 即其他线程调用waitingThread.unterrupt()就可以打断这个等待中的线程.

    被打断的线程会抛出InterruptedException.

## 几个概念

1. 可重入锁

    如果锁具备可重入性, synchronized和ReentrantLock都是可重入锁. 比如有两个被synchronized修饰的方法, 那么线程获得锁调用方法1是, 可以直接调用方法2, 而不用重新获得锁.

1. 可中断锁

    synchronized是不可中断锁, Lock是可中断锁. 即在block态的线程是否可以响应中断或者中断自身. lockInterruptibly()就提现了可中断性.

1. 公平锁

    公平锁尽量以请求锁的顺序来获得锁. synchronized是非公平锁, 无法保证等待的线程获得锁的顺序.

    ReentrantLock和ReentrantReadWriteLock默认是非公平锁, 但是可以设置为公平锁.

1. 读写锁

    读写锁将对资源的访问分成了读和写两种模式. 因为前文已经写过, 再次不再赘述.

1. 自旋锁

    当其他线程占用锁的时候, 不进入block态, 而是循环检测能否获得临界区锁.

1. 偏向锁

    大多数情况下不仅锁不存在多线程竞争, 而且总是由同一个线程多次获得, 为了让线程获得锁的代价更低, 因此引入了偏向锁. 当一个线程访问同步块并获取锁时, 会在对象头和栈帧的锁记录里存储锁偏向的线程id, 以后线程进入和退出同步块时不需要花费CAS操作来加锁和解锁, 只需要检查一下对象头的Mark Word里面是否存储着当前线程的偏向锁. 如果测试成功, 那么获得锁, 否则测试一下Mark Word中偏向锁的标志是否设置, 如果设置了, 那么尝试使用CAS将对象头的偏向锁设置为当前线程.

1. 轻量级锁

    在同步之前会在当前线程的栈帧中创建用于存储锁记录的空间, 并将对象头中的Mark Word复制到锁记录中, 然后线程尝试用CAS将对象头中的Mark Word替换为指向锁记录中的指针. 如果成功就获得锁, 否则通过自旋来获得锁.
