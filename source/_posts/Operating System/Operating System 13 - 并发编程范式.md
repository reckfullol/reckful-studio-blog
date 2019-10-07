---
title: Operating System 13 - 并发编程范式
date: 2019-10-06 17:26:15
categories: Operating System
---
# 并发编程范式

<!--more-->

## 并发编程概念

并发编程的三要素是:

1. 内存
2. 任务
3. 线程

并发编程就是关于如何抽象, 封装和操作三要素的艺术.

## 内存

并发编程的源头是在于内存中的数据需要在不同的线程之间共享, 因为多线程程序在运行时存在交错(interleaving).

数据在内存中的存储可以分为:

1. Immutable
2. Mutable
   1. 原子存储
   2. 固定存储
   3. 动态存储

## 任务

所有并发编程都是以任务为抽象的设计单元, 一个线程可以在他的生命周期里处理若干任务, 而任务间, 线程间总会存在着依赖关系.

我们将任务抽象成五个要素:

1. 前置任务依赖
2. 输入, 共享数据读
3. 执行的计算和操作
4. 输出, 共享数据写
5. 后置任务通知

## 线程

通常来说, 并发编程范式分为三种:

1. CSP(Communicating Sequentail Processes)
2. Actor(Functional)
3. Procedural

#### CSP

![](https://i.loli.net/2019/10/06/L9OxeDSbz7MNYPj.png)

CSP是由Tony Hoare在1978的[论文](http://www.usingcsp.com/cspbook.pdf)上首次提出的. CSP将程序分为Processor和Channel两个部分:

1. Processor
   
   任务执行单元, 内部没有并发.

2. Channel

    Processor间的信息交互媒介, 负责共享数据的交换, 修改, 消息传递等.

除了Channel, Processor间没有其他联系, 这样就将并发同步作用缩小在了Channel, 问题规模得到了规约. CSP使得系统较为清晰, Processor之间解耦, 职责明晰.

CSP规范了:

1. 工作者之间不直接进行通信.
2. 工作者向不同的通道中发布自己的消息(事件), 其他工作者可以在这些通道上监听信息, 发送者不知道具体是谁在执行(匿名).
3. 消息交互是同步的.

CSP的设计哲学是:

> Do not communicate by sharing memory, instead, share memory by communicating.

并发编程不要利用共享内存来进行线程通信, 而应该依靠通讯来共享数据. 尽量避免锁和线程争用.

#### Actor(Functional)

![](https://i.loli.net/2019/10/06/KTp5Ag1ZDHIXRCu.png)

Actor模型是由Carl Hewitt于1973年提出, 后由Erlang OTP提出. Actor属于并发组件模型, 通过组件方式定义并发编程范式的高级阶段, 避免使用者直接接触多线程并发或线程池等基础概念, 其消息传递更加符合面向对象的原始意图.

传统多数流行的语言并发是基于多线程之间的共享内存, 使用同步机制来防止写争夺. 而Actor使用消息模型, 每个Actor在同一时间处理最多一个消息, 可以发送消息给其他Actor, 保证来单独写原则.

Actor模型不仅仅对于单机的并发应用开发有意义, 对于分布式应用的开发也是一个可以大展手脚的场景: 节点之间相互独立, 只能靠消息通讯, 异步消息避免节点瓶颈等特性都非常贴合Actor都使用.

Actor模型特点:

1. 万物都是Actor.
2. Actor之间完全独立, 只允许消息传递, 不允许其他任何共享.
3. 每个Actor最多同时只能进行一样工作.
4. 每个Actor都有一个专属的明明MailBox(非匿名).
5. 消息的传递完全异步.
6. 消息不可变.

#### Procedural

以C语系为代表的过程式编程语言, 在处理并发编程时, 通常是使用同步工具来完成, 这些工具按照抽象级别分为:

1. BlockingQueue, TaskQueue, Producer-Consumer Queue, CountDownLatch, Reader-Writer Lock等.
2. mutex, conditiona variable, future，衍生出去还有：shared_future, promise, lock guard, unique lock等.
3. lock-free, atomic, spin lock.
