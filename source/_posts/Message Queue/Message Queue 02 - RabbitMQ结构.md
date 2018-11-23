---
title: Message Queue 02 - RabbitMQ结构
date: 2018-11-03 22:18:04
categories: Message Queue
---
# RabbitMQ结构

<!--more-->

![rabbit_architecture](https://res.cloudinary.com/dpe4i978o/image/upload/v1541251536/mq/rabbit_architecture.png)

1. Message

    message是不具名的, 由消息头和消息体组成. 消息体是不透明的, 而消息头由一系列可选属性构成, 包括routing-key, priority, delivery-mode等.

2. Publisher

    消息的生产者, 也是一个向交换器发布信息的客户端应用程序.

3. Exchange

    交换器, 用来接收生产者发送的消息并将这些消息路由给虚拟主机中的队列.

4. Binding

    绑定, 用于关联消息队列和交换器. 绑定就是基于路由键将交换器和消息队列连接起来的路由规则, 所以可以将交换器理解成一个由Binding构成的路由表.

5. Queue

    消息队列, 用来保存信息直到发给消费者. 他是消息的容器, 也是消息的终点. 一个消息可投入一个或多个消息队列. 消息一直在队列里, 等待消费者链接到这个队列将其取走.

6. Connection

    网络连接, 比如一个TCP连接.

7. Channel

    信道, 多路复用连接中的一条独立的双向数据流通道. 信道是建立在真实的TCP连接的虚拟连接, AMQP命令都是通过信道发出去的, 不管是发布信息, 订阅队列还是接受信息, 这些动作都是通过信道完成. 因为对于操作系统来说建立和销毁TCP都是非常昂贵的开销, 所以引入了信道的概念, 用来复用一条TCP连接.

8. Consumer

    消息的消费者, 表示一个从消息队列中取得消息的应用程序.

9. Virtual Host

    虚拟主机, 用来表示一批交换器, 消息队列和相关对象. 虚拟主机是共享相同的身份认证和加密环境的独立服务器域. 每个vhost本质上就是一个mini版的RabbitMQ服务器, 拥有自己的队列, 交换器, 绑定和权限机制. vhost是AMQP概念的基础, 必须在连接时指定, RabbitMQ默认的vhost是/.

10. Broker

    表示消息队列服务器实体.