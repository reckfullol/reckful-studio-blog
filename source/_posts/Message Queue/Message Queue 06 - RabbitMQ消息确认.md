---
title: Message Queue 06 - RabbitMQ消息确认
date: 2018-11-26 22:54:05
categories: Message Queue
---
# RabbitMQ消息确认

<!--more-->

![amqp_rpc](https://res.cloudinary.com/dpe4i978o/image/upload/v1543282707/mq/amqp_rpc.png)

在我们使用RabbitMQ过程中, 无法感知消息是否正确的到达broker. 如果不进行配置的话, 默认情况是不会返回任何信息给生产者的. 因此RabbitMQ提供了三种方法来进行消息的确认:

## Transaction

RabbitMQ与事务机制有关的三个方法:

1. txSelect(): 用于将当前channel设置为transaction模式.
2. txCommit(): 用于提交事务.
3. txRollback(): 用于回滚事务.

但是在开始事务模式的情况下, RabbitMQ的时延和吞吐量都有显著的影响, 因此假如不是必要的话, 尽量避免使用事务机制.

## Confirm

生产者将channel设置为confirm模式, 一旦channel进入confirm模式, 所有在该channel上发布的信息都会被指派一个唯一的ID(从1开始), 一旦消息被投递到所有匹配的队列之后, broker就会发送一个确认给发送者(包含消息的唯一ID), 这就使得生产者将消息正确的发送到了broker. 如果消息和队列是持久化的, 那么确认消息会在消息写入磁盘后发出. broker回传给生产者的确认消息中delivery-tag域中包含了确认消息的序列号, 此外broker也可以设置basic.ack的mulitple域, 表示到这个序列号之前的消息都已经得到了处理.

confirm模式的好处在于异步, 一旦发布一条消息, 生产者应用程序就可以在等待信道返回确认的同时继续发送下一条消息, 当消息最终得到确认之后, 生产者应用便可以通过毁掉方法来处理该确认消息. 如果RabbitMQ因自身内部错误导致消息丢失, 就会发送一条nack消息, 生产者应用程序同样可以在回调方法中处理该nack消息.

在channel被设置成confirm模式之后, 所有被publish的后续消息豆浆杯confirm或者被nack一次. 但是没有对消息confirm的快慢做任何保证, 并且同一条消息不会既被confirm又被nack.

## RPC

一般来说通过RabbitMQ来实现RPC是比较容易的. 一个客户端发送请求队列, 服务器端将其应用到一个回复信息中. 为了接受到回复消息, 客户端需要在发送请求的时候同时发送一个回调队列(callback queue)的地址.

```python
result = channel.queue_declare(exclusive=True)
callback_queue = result.method.queue

channel.basic_publish(exchange='',
                      routing_key='rpc_queue',
                      properties=pika.BasicProperties(
                            reply_to = callback_queue,
                            ),
                      body=request)

# ... and some code to read a response message from the callback_queue ...
```

### 消息属性

AMQP协议给消息预定义了一系列的14个属性, 以下几个较为常用:

1. delivery_mode(投递模式): 将消息标记为持久化(值为2)或者暂存(除2以外的任何值).
2. content_type(内容类型): 用来描述编码的mime-type.
3. reply_to(回复目标): 通常用来命名回调队列.
4. correlation_id(关联标识): 用来将RPC的响应和请求关联起来.

### 关联标识

上述方法中, 每一个RPC都会请求新建一个回调队列, 更高效的方法是为每一个客户端建一个独立的回调队列. 但是此队列接收到一个响应的时候无法辨别出这个相应是来自于哪个请求. 因此correlation_id就可以将响应和请求匹配起来. 如果我们接手的correlation_id是未知的, 那就直接销毁掉, 因为他不属于我们的任何一条请求.

接受到一条未知消息的时候不抛出错误, 而是将他忽略掉是源于解决服务端有可能发生的竞争情况. 尽管可能性不大, 但是RPC服务器还是有可能在已将应答发送给我们但还未将确认消息发送给请求方的时候宕掉. 如果发生这种情况, RPC服务器会在重启后重新请求, 这就是为什么客户端需要优雅的处理重复相应, 同时也要尽量保证幂等性.

## 注意事项

当一个问题被抛出的时候, 我们往往意识不到是本地调用还是由较慢的RPC调用引起的, 同时这使得系统具有不可预测性和给调试工作带来不必要的复杂性. 而且滥用RPC会导致不可维护的面条代码.

因此我们要确保能够明确哪个函数是本地调用的, 哪个函数是远程调用的, 保证各个组建间的依赖明确, 明确客户端如何处理RPC服务器的宕机和长时间无响应的情况.

如果在使用RPC有疑问, 可以使用异步管道来代替RPC的阻塞.