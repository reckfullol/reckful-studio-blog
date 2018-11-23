---
title: Message Queue 03 - AMQP中的消息路由
date: 2018-11-03 23:28:26
categories: Message Queue
---
# AMQP中的消息路由

<!--more-->

![AMQP_routing](https://res.cloudinary.com/dpe4i978o/image/upload/v1541337053/mq/AMQP_routing.png)

AMQP的消息路由和JMS的差别在于, AMQP中添加了Exchange和Binding角色, 生产者把消息发布到Exchange上, 消息最终到达队列被消费者接受, 而Binding决定Exchange的消息应该发送到哪个队列.

## AMQP架构

![AMQP_architecture](https://res.cloudinary.com/dpe4i978o/image/upload/v1541489322/mq/AMQP_architecture.png)

AMQP是一个异步消息传递所使用的应用层协议规范, AMQP客户端能够无视消息来源任意发送和接受消息. Broker提供消息的路由, 队列等功能. Broker主要由Exchange和Queue来构成: Exchange负责接受消息, 转发消息到绑定的队列, Queue存储消息, 提供持久化, 队列等功能. AMQP客户端通过Channel与Broker通信, Channel是多路复用连接中的一条独立的双向数据流通道.

## Exchange类型

Exchange分发消息时根据类型的不同分发策略有区别, 目前共有四种类型: direct. fanout, topic, headers. 要注意的是, headers匹配AMQP消息的header而不是routing-key, 此外headers交换器和direct交换器完全一致, 但性能差很多, 所以下面只讨论其余三类.

1. direct

    ![AMQP_direct](https://res.cloudinary.com/dpe4i978o/image/upload/v1541340802/mq/AMQP_direct.png)

    消息中的routing-key如果和binding中的binding-key一致, 那么exchange就将消息发送到对应的队列中. 如果一个队列的binding-key为"dog", 那么exchange只转发routing-key为"dog"的message, 不会转发"dog.puppy". 他是完全匹配, 单播模式.

2. fanout

    ![AMQP_fanout](https://res.cloudinary.com/dpe4i978o/image/upload/v1541340803/mq/AMQP_fanout.png)

    每个发到fanout类型exchange的message都会分发到所有绑定的队列上去. fanout exchange不会处理routing-key, 只是简单的将队列绑定到exchange上. 这很像子网广播, 每台子网的主机都获得了一份复制的消息. fanout类型转发消息是最快的.

3. topic

    ![AMQP_topic](https://res.cloudinary.com/dpe4i978o/image/upload/v1541340804/mq/AMQP_topic.png)

    topic exchange通过模式匹配message的routing-key, routing-key要求必须是由点隔开的一系列标识符. binding-key有两种特殊情况, "\*"可以匹配一个标识符, "\#"可以匹配0个或多个标识符.