---
title: Message Queue 07 - RabbitMQ持久化
date: 2018-11-27 10:18:59
categories: Message Queue
---
# RabbitMQ持久化

<!--more-->

RabbitMQ默认是不持久化queue, exchange, binding以及queue中的消息. 这意味着一旦消息服务器宕机, 所有已声明的结构和消息都会丢失.

那么我们通过设置exchange和queue的durable属性为true, 才能使得queue和exchange持久化, 但是这还不能让queue中的消息持久化. 这需要生产者在发送消息的时候. 将delivery_mode设置为2, 只有这三个设置都完成, 才能保证服务器重启不会对现有结构和消息造成影响.

需要注意的是, 只有durable为true的exchange和durable为true的queue才能绑定, 否则在绑定的时候, RabbitMQ会报错.