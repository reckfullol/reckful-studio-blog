---
title: Networks 15 - IP分片
date: 2018-12-04 16:00:09
categories: Networks
---
# IP分片

<!--more-->

![ip_fragmentation](https://res.cloudinary.com/dpe4i978o/image/upload/v1543910340/internet/ip_fragmentation.png)

在数据链路层中MTU(Maximum Transmission Unit)来限制一次传输的数据最大长度, 那么当IP数据包大小超过MTU时, 就需要进行分片.

## 原理

IP分片发生在IP蹭, 不仅源主机会进行分片, 中间路由器也有可能会进行分片, 因为不同的网络MUTU不同. 因此在传输链路上路由器们可能会对IP数据包进行多次分片, 而分片数据的重组只会发生在目的端的IP层.

IP首部由4个字节用于分片, 16位用于给IP数据包标志, 13位标识字节偏移数, 3位标识中有1位表示后面是否还有分片/

## 避免分片

IP层没有超时重传机制, 如果IP层对一个数据包进行分片, 只要有一个分片丢失, 就需要依赖传输层进行重传, 结果是所有的分片都要重传一遍, 因此我们要尽量避免IP分片.

对于UDP包, 我们需要在应用层限制包的大小, 一般不超过1472字节, 即以太网MTU(1500) - UDP首部(8) - IP首部(20). 事实上以太网MTU的1500字节仅供参考, 具体需要根据链路上的路由器而定.

对于TCP包就不需要考虑这个问题, 因为在三次握手的过程中, 连接双方已经告知了MSS(Maximum Segment Size), MSS一般是MTU - IP首部(20) - TCP首部(20), 每次发送的TCP数据都不会超过双方的MSS最小值, 避免了IP分片.