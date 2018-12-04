---
title: Networks 16 - 字节序
date: 2018-12-04 20:46:03
categories: Networks
---
# 字节序

<!--more-->

字节序分为大端字节序(big endian)和小端字节序(little endian).

大端字节序是指一个整数的高位字节存储在内存的低地址处, 小端字节序是指一个整数的高位字节存储在内存的高地址处.

我们可以通过下面的方式去查看本机的字节序.

```c
union {
    char a[4];
    int b;
}
```

现在的PC大多采用小端字节序, 因此小端字节序也被称为主机字节序.

当我们把数据发送到不同字节序主机的时候, 需要建立一定的约定, 保证对方解析数据正确. 所以我们约定在网络中传递的时候都是使用大端字节序, 因此大端字节序也被称为网络字节序, 他给所有接受数据的主机提供了一个正确解释收到的格式化数据的保证.

特别注意的是, 即使是一台主机上的两个进程通信, 也要考虑字节序的问题, 比如说JVM就是采用的大端字节序.

Linux提供了如下的四个函数来完成主机字节序和网络字节序之间的转换:

```c
#include <netinet/in.h>
unsigned long int htonl(unsigned long int hostlong);
unsigned short int htons(unsigned short int hostshort);
unsigned long int htonl(unsigned long int netlong);
unsigned short int htonl(unsigned short int netshort);
```

长整型函数常用来转换IP地址, 短整形常用来转换端口号.