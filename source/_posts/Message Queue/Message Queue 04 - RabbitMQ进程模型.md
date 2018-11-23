---
title: Message Queue 04 - RabbitMQ进程模型
date: 2018-11-06 15:43:10
categories: Message Queue
---
# RabbitMQ进程模型

<!--more-->

![rabbitmq_process](https://res.cloudinary.com/dpe4i978o/image/upload/v1541489591/mq/rabbitmq_process.png)

RabbitMQ Server实现了AMQP模型中Broker部分, 将Channel和Queue设计成了Erlang进程, 并用Channel进程的运算实现了Exchange的功能.

tcp_acceptor进程接收客户端连接, 创建rabbit_reader, rabbit_writer, rabbit_channel进程. rabbit_reader接受客户端连接, 解析AMQP帧; rabbit_writer向客户端返回数据; rabbit_channel解析AMQP方法, 对消息进行路由, 然后发送给相应的队列进程. rabbit_amqqueue_process是队列进程, 在RabbitMQ启动(恢复durable类型队列)或创建队列时创建. rabbit_msg_store是负责消息持久化的进程.

在整个系统中, 存在一个tcp_accepter进程, 一个rabbit_msg_store进程, 有多少个队列就有多少个rabbit_amqqueue_process进程, 每个客户端连接对应一个rabbit_reader和rabbit_writer进程.