---
title: Networks 09 - SSL握手
date: 2018-08-03 13:36:56
categories: Networks
---
# SSL握手

<!--more-->

## 过程

![fuck](https://res.cloudinary.com/dpe4i978o/image/upload/v1533274463/internet/SSL.png)

1. 客户端给出协议版本号, 客户端生成的随机数(client random), 以及客户端支持的加密方式.

2. 服务器确认双方使用的加密方法, 并给出数字证书, 以及一个服务器生成的随机数(server random).

3. 客户端确认数字证书有效, 并生成一个新的随机数(premaster secret), 并使用数字证书中的公钥, 加密这个随机数, 发给服务器.

4. 服务器用自己的私钥, 来解密客户端发出的随机数(premaster secret).

5. 客户端和服务器根据前面三个随机数, 生成对话密钥(session key), 来加密接下来的整个对话过程.

## 注意

1. 生成对话密钥一共需要三个随机数.

2. 握手之后的对话使用对话密钥(session secret)加密, 是对称加密. 服务器的公钥和私钥只用于加密和解密对话密钥, 是非对称加密, 没有其他作用.

3. 服务器的公钥存放在服务器的数字证书中.

4. 整个握手阶段都不加密(也没法加密), 是明文传输的. 因此如果有人偷听到了, 就可以知道双方的加密方式和两个随机数(client random 和 server random). 整个通话的安全在于能否破解被公钥加密过的premaster secret. 因此可以采用DH密钥交换算法, 这样premaster secret就不需要传递, 只需要双方交换各自参数, 就可以算出这个随机数.