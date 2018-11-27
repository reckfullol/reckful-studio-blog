---
title: Message Queue 08 - RabbitMQ集群
date: 2018-11-27 10:50:27
categories: Message Queue
---
# RabbitMQ集群

<!--more-->

## 设计集群的目的

1. 允许消费者和生产者在RabbitMQ节点崩溃的情况下继续运行.
2. 通过增加更多的节点来扩展通信消息的吞吐量.

## 集群配置方式

1. cluster:
    - 不支持跨网段, 用于同一个网段内的局域网.
    - 可以动态增加或减少.
    - 节点间需要运行相同版本的RabbitMQ和Erlang.

2. federation:

    应用于广域网, 允许单台服务器上的exchange或queue接受发布到另一个服务器上exchange或queue的队列, 可以是单独机器或集群. federation队列类似于单向点对点连接, 消息会在联盟队列之间转发任意次, 知道被消费者接受. 通常用federaion来连接internet上的中间服务器, 用来当作订阅分发消息或工作队列.

3. shovel:

    连接方式与federation的连接方式类似, 但他的工作在更低层次, 可以应用于广域网.

## 节点类型

- RAM node:

    内存节点将所有的queue, exchange, binding, 用户, 权限和vhost元数据存储在内存中, 好处是是可以使得像queue, exchange声明等操作更加的快速.

- Disk node:

    将元数据存储在磁盘中, 单节点系统中只允许磁盘类型的节点, 防止重启RabbitMQ的时候, 丢失系统的配置信息.

> 问题说明: RabbitMQ要求在集群中至少有一个磁盘节点, 其他所有节点可以是内存节点, 当节点加入或者离开集群时, 必须要将该变更通知到至少一个磁盘节点. 如果集群中唯一的一个磁盘节点崩溃的话, 集群仍然可以保持运行, 但是无法进行其他操作(增删改查), 直到节点回复.

> 解决方案: 设置两个磁盘节点, 至少有一个是可用的, 可以保存元数据的更改.

## Erlang Cookie

Erlang Cookie是保证不同节点可以互相通信的密钥, 要保证集群中的不同节点相互通信必须共享相同的Erlang Cookie, 具体的目录存放在`/var/lib/rabbitmq/.erlang.cookie`.

> 说明: 这就要从rabbitmqctl命令的工作原理说起, RabbitMQ底层是通过Erlang架构来实现的, 所以rabbitmqctl会启动Erlang节点, 并基于Erlang节点来使用Erlang系统连接RabbitMQ节点, 在连接过程中需要正确的Erlang Cookie和节点名称, Erlang节点通过交换Erlang Cookie来获得认证.


## 镜像队列

RabbitMQ的Cluster集群一般分为两种, 普通模式和镜像模式.

- 普通模式:

    默认的集群模式, 以两个节点（rabbit01、rabbit02）为例来进行说明. 对于Queue来说, 消息实体只存在于其中一个节点rabbit01（或者rabbit02）, rabbit01和rabbit02两个节点仅有相同的元数据, 即队列的结构. 当消息进入rabbit01节点的Queue后, consumer从rabbit02节点消费时, RabbitMQ会临时在rabbit01、rabbit02间进行消息传输, 把A中的消息实体取出并经过B发送给consumer. 所以consumer应尽量连接每一个节点, 从中取消息. 即对于同一个逻辑队列, 要在多个节点建立物理Queue. 否则无论consumer连rabbit01或rabbit02, 出口总在rabbit01, 会产生瓶颈. 当rabbit01节点故障后, rabbit02节点无法取到rabbit01节点中还未消费的消息实体. 如果做了消息持久化, 那么得等rabbit01节点恢复, 然后才可被消费；如果没有持久化的话, 就会产生消息丢失的现象.

    ![amqp_queue_1](https://res.cloudinary.com/dpe4i978o/image/upload/v1543288962/mq/amqp_queue_1.png)

    下面表示在集群配置下的不同节点创建队列的情况

    ![amqp_queue_2](https://res.cloudinary.com/dpe4i978o/image/upload/v1543288962/mq/amqp_queue_2.png)
    
    下图表示在集群配置下的不同节点创建交换器和队列的绑定的情况
    
    ![amqp_queue_3](https://res.cloudinary.com/dpe4i978o/image/upload/v1543288962/mq/amqp_queue_3.png)

- 镜像模式:

    将需要消费的队列变成镜像队列, 存在于多个节点, 这样就可以实现RabbitMQ的HA高可用性. 作用就是消息实体会主动在镜像节点之间实现同步, 而不是像普通模式那样, 在消费者消费数据时临时读取. 缺点是集群内部通讯会占用大量的带宽.

    ![amqp_queue_4](https://res.cloudinary.com/dpe4i978o/image/upload/v1543288962/mq/amqp_queue_4.png)