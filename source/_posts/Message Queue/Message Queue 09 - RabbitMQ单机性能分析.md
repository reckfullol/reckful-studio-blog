---
title: Message Queue 09 - RabbitMQ单机性能分析
date: 2019-03-13 22:00:41
categories: Message Queue
---
# RabbitMQ单机性能分析

<!--more-->

## Broker配置

- CPU: Intel(R) Xeon(R) CPU E5-2620 v3 @ 2.40GHz
- 内存: 35GB
- Erlang: Erlang (BEAM) emulator version 10.1.1
- RabbitMQ: 3.7.8
- OS: Tencent tlinux release 1.2 (Final)

## 分析目标

1. **时延**: 针对于游戏类强互动型业务, **响应时间**是十分影响玩家观感的重要指标, 因此是我们分析的重点.
2. **吞吐量**: 针对于游戏业务的特点, 集中访问是经常出现的场景, 因此需要依赖MQ进行**流量削峰**, 承载大量请求, 保证较高的**吞吐量**.
3. **机器负载**


## 测试模型

![performance_test_architecture](https://res.cloudinary.com/dpe4i978o/image/upload/v1551923439/mq/performance_test_architecture.png)

我们从 **Send** 端发送消息一条带有**时间戳**的消息至RabbitMQ, 然后消息转发到 **Reply** 端, **Reply** 将标志位修改, 表示完成转发操作, 再将消息发送回 **RabbitMQ**, 最终回到 **Recv** 端, 将时间戳于当前时间做差, 得到这一次链路所需时间. 为了保证时间的准确性, 我们可以把 **Send** 和 **Recv** 部署到同一个实体机.

[测试代码](https://github.com/Snakeflute/rabbitmq-performance-test)


## 性能测试

### 横向比较

我们设置发包大小为 **1K Bytes**, 分别控制发包速率为 **3w/s**, **4w/s**, **5w/s**, **6w/s**下, 消息的**时延分布**和 **broker 负载**:

#### 链路时延

- 3w/s:
  
    ![3w_1k](https://res.cloudinary.com/dpe4i978o/image/upload/v1552446299/mq/3w_1k_probability_distributions.png)

    此时 **99.5%** 的链路时延都在 **20ms** 以下, **0.05%** 超过 **20ms**, 最大时延在 **40ms** 左右.

- 4w/s:
  
    ![4w_1k](https://res.cloudinary.com/dpe4i978o/image/upload/v1552445960/mq/4w_1k_probability_distributions.png)

    此时 **99%** 的链路时延都在 **20ms** 以下, **1%** 超过 **20ms**, 最大时延在 **120ms** 左右.

- 5w/s:
  
    ![5w_1k](https://res.cloudinary.com/dpe4i978o/image/upload/v1552445960/mq/5w_1k_probability_distributions.png)

    此时**98%**链路时延都在**20ms**以下, **2%**超过**20ms**, 最大时延在**200ms**左右.

- 6w/s:
  
    ![6w_1k](https://res.cloudinary.com/dpe4i978o/image/upload/v1552445961/mq/6w_1k_probability_distributions.png)

    此时 **96%** 链路时延都在 **20ms** 以下, **4%** 超过 **20ms**, 最大时延在 **150ms** 左右.

#### Broker 负载

![landscape_load](https://res.cloudinary.com/dpe4i978o/image/upload/v1552465797/mq/landscape_load.png)

CPU负载和上行带宽负载和发包速率呈**线性关系**, 内存负载**没有变化**.

### 纵向比较

我们再控制 **4w/s** 发包速率下发包大小分别为 **500 Bytes**, **1K Bytes**, **2K Bytes** 的消息**时延分布**和 **broker 负载**:

#### 链路时延

- 500 Bytes:
- 
    ![4w_500byte](https://res.cloudinary.com/dpe4i978o/image/upload/v1552445961/mq/4w_500b_probability_distributions.png)

    此时 **99%** 链路时延都在 **20ms** 以下, **1%** 超过 **20ms**, 最大时延在 **40ms** 左右.
    

- 1K Bytes:
  
    ![4w_1k](https://res.cloudinary.com/dpe4i978o/image/upload/v1552445960/mq/4w_1k_probability_distributions.png)

    此时 **99%** 链路时延都在 **20ms** 以下, **1%** 超过 **20ms**, 最大时延在 **120ms** 左右.

- 2K Bytes:
  
    ![4w_2k](https://res.cloudinary.com/dpe4i978o/image/upload/v1552445960/mq/4w_2k_probability_distributions.png)

    此时 **98%** 链路时延都在 **20ms** 以下, **2%** 超过 **20ms**, 最大时延在 **200ms** 左右.

#### Broker 负载

![portrait_load](https://res.cloudinary.com/dpe4i978o/image/upload/v1552466529/mq/portrait_load.png)

CPU负载和上行带宽负载和发包速率呈**线性关系**, 内存负载**没有变化**.

## 性能分析

  1. 时延: RabbitMQ的时延在绝大时候都能维持在链路时延 **20ms**, 单向时延 **10ms** 下, 但是我们注意到, 消息的时延会存在规律性的波峰, 对照这个时期的CPU负载和内存负载:
      
      ![cpu_fluctuation](https://res.cloudinary.com/dpe4i978o/image/upload/v1552469319/mq/cpu_fluctuation.png)

      ![mem_fluctuation](https://res.cloudinary.com/dpe4i978o/image/upload/v1552469504/mq/mem_fluctuation.png)
      
      我们可以发现此时CPU的负载有明显的下降, 而内存并没有波动. 说明此时并没有发生 **paging** (消息持久化到磁盘), 因为我们的消息也并没有持久化设置. 因此**推测**, 这时可能是该节点的Erlang GC按照进程级别进行标记-清扫, 会将 **amqqueue** 进程暂停, 直至GC结束. 由于在RabbitMQ中, 一个队列只有一个 **amqqueue** 进程, 该进程又会处理大量的消息, 产生大量的垃圾, 这就导致了该进程GC较慢, 进而流量控制block上游, 导致了消息时延出现较大波动.

  2. 吞吐量: 发包速率在 **4w/s(500 Bytes)** 即RabbitMQ吞吐量 **8w/s(500 Bytes)**, CPU负载在 **88%**, 链路时延 **20ms** 以下的比率在 **99%** 左右, 最大时延在 **40ms** 左右. 我们可以发现, RabbitMQ的**吞吐量**和 **CPU 负载**成**线性关系**.

## 优化

1. hipe_compile
   
    在 **/etc/rabbitmq/rabbitmq.config** 我们可以将 **hipe_compile** 选项打开:

    ```
    Explicitly enable/disable HiPE compilation.
    {hipe_compile, true}
    ```

    其原理类似于 **JIT**, 可以对RabbitMQ进行预编译, 以增加启动时间为代价降低CPU的负载, 进而提高RabbitMQ的吞吐量, 我们还是以  **4w/s(1K Byte)** 的发包速率进行测试:

    ![hipe_compile](https://res.cloudinary.com/dpe4i978o/image/upload/v1552471264/mq/cpu_compare.png)

    我们可以看到, CPU的负载降低了 **23%**, 是十分可观的提升.

2. 及时取出消息

    在RabbitMQ中, 如果生产者持续高速发送, 而消费者速度较低, 此时会触发流量控制限制消费者的发送, 直到消费者完成消息的取出. 而这一过程会造成**消息时延过高**, 因此需要及时取出消息.

3. 横向扩展

    当单个Broker无法满足业务的qps时, 可以通过设置 **RabbitMQ Cluster** 的方式横向扩展吞吐量, 并且可以采用镜像队列的方式实现高可用. 值得注意的是, 镜像队列的方式会在各个Broker间同步消息, 占用Broker带宽和CPU.

## 总结

我们可以看到, 在吞吐量为 **8w/s(发包速率4w/s, 500 Bytes)** 时, CPU的负载为 **71%**, 时延整体在 **10ms** 以下, 作为服务于游戏业务的中间件时, 其低时延, 高吞吐的特性是可以满足一般的场景业务需求.

## 注

> 链路时延: 一条消息从 send -> rabbitmq -> reply -> rabbitmq -> recv 所需要的时间, 单向时延 = 链路时延 / 2.