---
title: Networks 12 - 判断TCP包是否发送成功
date: 2018-08-23 23:49:20
categories: Networks
---
# 判断TCP包是否发送成功

<!--more-->

## send()

对发送端而言, 用户空间调用send(data)等发送接口将数据发送, 内核会将data拷贝到内和空间的socket对应的缓冲中, 而send()函数的返回值仅仅是表示本次send()调用中成功拷贝的字节数, 具体的发送和接收端的接收就由TCP协议完成. 

## 判定是否接收成功

1. 查看接收端是否回复应答消息.

2. 计算发送端socket已发送数量, 利用接口`ioctl(tcp_socket, SIOCOUTQ, &value)`.