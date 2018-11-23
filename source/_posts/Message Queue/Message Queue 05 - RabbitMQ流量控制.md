---
title: Message Queue 05 - RabbitMQ流量控制
date: 2018-11-06 16:08:00
categories: Message Queue
---
# RabbitMQ流量控制

<!--more-->

![rabbitmq_messgae_path](https://res.cloudinary.com/dpe4i978o/image/upload/v1541491006/mq/rabbitmq_messgae_path.png)

RabbitMQ可以对内存和磁盘使用量设置阈值, 当达到阈值时, 生产者将被阻塞, 直到对应项恢复正常. 除了这两个阈值, RabbitMQ在正常情况下还用流量控制机制来确保稳定性.

Erlang进程之间并不共享内存(binaries类型除外), 而是通过消息传递来通信, 每个进程都有自己的进程邮箱. Erlang默认没有对进程邮箱大小设限制, 所以当有大量消息持续发往某个进程时, 会导致该进程邮箱过大, 最终内存溢出并崩溃.

在RabbitMQ中, 如果生产者持续高速发送, 而消费者消费速度较低时, 如果没有流量控制, 很快就会使内部进程邮箱大小达到内存阈值, 阻塞生产者(而不是崩溃). 然后RabbitMQ会进行page操作, 将内存中的数据持久化到硬盘中.

因此, RabbitMQ使用了一种基于信用的流量控制机制. 消息处理进程有一个信用组{InitialCredit, MoreCreditAfter}, 默认值为{200, 50}. 消息发送者进程A向接收者进程B发送消息, 每发一条消息, Credit数量减1, 直到为0, 被挂起. 对于接收者B, 每接受MoreCreditAfter条消息, 就会给A增加MoreCreditAfter个Credit, 当Credit > 0时, A可以继续向B发送消息.

可以看出基于信用的流量控制最终可以将消息发送速度限制在消息处理进程的处理速度内. RAbbitMQ中与流量控制有关的进程构成了一个有向无环图.