---
title: Operating System 12 - Reactor与Proactor
date: 2018-12-18 21:18:52
categories: Operating System
---
# Reactor与Proactor

<!--more-->

## Reactor

![reactor](https://res.cloudinary.com/dpe4i978o/image/upload/v1545138936/IO/reactor.png)

Reactor是基于同步I/O的, 他要求主线程(I/O处理单元)只负责监听文件描述符上是否有事件发生, 有的话就立刻将该事件通知给工作线程(逻辑单元). 初次之外, 主线程不做任何其他实质性的工作. 读写数据, 接受新的连接, 以及处理客户请求均在工作线程中完成.

1. 主线程往epoll内核事件表上注册socket的读就绪事件.
2. 主线程调用epoll_wait等待socket上有数据可读.
3. 当socket上读就绪后, epoll_wait通知主线程, 主线程将这一读就绪事件放入请求队列.
4. 某一工作线程被唤醒, 从socket上读取数据, 并进行逻辑运算, 并将要发送的数据从用户空间拷贝到内核发送缓冲区, 再在epoll上注册该socket写就绪事件.

## Proactor

![proactor](https://res.cloudinary.com/dpe4i978o/image/upload/v1545138893/IO/proactor.png)

Proactor是基于异步I/O的, 他将所有的I/O操作都交给主线程和内核来处理, 工作线程仅仅负责业务逻辑.

1. 主线程调用aio_read函数向内核注册socket的读就绪事件, 并告诉内核读缓冲区(用户空间)的位置, 以及读操作完成后如何响应(回调函数/信号处理).
2. 当内核接收缓冲区到用户缓冲区异步拷贝完后, 内核通知用户事件完成.
3. 应用程序响应事件, 选择一个工作线程来进行逻辑运算. 之后调用aio_write向内核注册socket写就绪事件, 并告诉内核写缓冲区(用户空间)的位置, 以及写操作完成后如何响应(回调函数/信号处理).
4. 当用户缓冲区的数据拷贝到内核发送缓冲区后, 内核通知用户事件完成, 用户响应事件.

这种异步模式的典型事件是基于操作系统底层异步API的, 可以称之为"系统级别"或者"真正意义上"的异步, 因为具体的IO操作实际上是由内核完成的.

但是并不是所有操作系统都为底层异步提供健壮的支持, 因此设计出一种通用统一的外部接口是非常困难的.