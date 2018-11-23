---
title: Ethereum 02 - 外部账户
date: 2018-07-01 23:14:40
categories: Ethereum
---
# 外部账户

<!--more-->

外部账户(EOA)由私钥来控制, 是由用户实际控制的账户. 每个外部账户有用一对公钥私钥, 这对密钥用于签署交易, 它的地址是由公钥决定. 外部账户不能包含以太坊虚拟机(EVM)代码. 我们可以把外部账户看作是用户在银行办理的一个账户, 公钥就是用户为该账户设置的卡号, 而私钥就是用户设置的密码. 一个外部账户具有以下特性:

1. 拥有一定的账户余额

2. 可以发送交易

3. 通过私钥控制

4. 没有相关的代码

用户可以使用Geth(以太坊客户端)指令创建一个外部账户, 生成一个账户地址的过程主要有三步:

1. 设置用户的私钥, 也就是通常意义的用户密码

1. 使用加密算法由私钥生成对应的公钥

1. 根据公钥得出相应的账户地址

其中第二步使用的加密算法是secp256k1椭圆曲线密码算法, 而不是RSA加密算法, 因为前者相较于后者更加高效安全. 对于由公钥得到账户地址, 在以太坊中使用SHA3方法. 我们通过JavaScript的CryptoJS加密库来演示生成以太坊账户地址的过程:

```js
// pubKey -> address
var pubKeyWordArray = CryptoJS.enc.Hex.parse(pubKey);
var hash = CryptoJS.SHA3(pubKeyWordArray, {outputLength : 256});
var address = hash.toString(CryptoJS.enc.Hex).slice(24)
```