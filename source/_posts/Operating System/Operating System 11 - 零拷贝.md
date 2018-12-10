---
title: Operating System 11 - 零拷贝
date: 2018-12-06 20:29:14
categories: Operating System
---
# 零拷贝

<!--more-->

## legacy

当我们将服务端主机磁盘中的文件不做修改的从已连接的socket发送出的, 通常是这么做的:

```c
while(ret = read(disk_fd, buf, BUF_SIZE) > 0) {
    write(sock_fd, buf, ret);
}
```

也就是说我们将磁盘中的信息读入到内存中, 再将内存中的信息发送到socket. 但是由于Linux的I/O操作是基于缓冲的. 也是就是说在以上的I/O中, 发生了多次的数据拷贝.

![send_file_legacy](https://res.cloudinary.com/dpe4i978o/image/upload/v1544098782/linux/send_file_legacy.jpg)

当应用程序访问某块数据的时候, 会首先检测最近是否有过访问, 文件内容是否在内核中存在缓存. 如果是, 操作系统则直接根据read系统调用提供的buffer地址, 将buffer所指定的数据拷贝到用户态的空间中. 如果不是, 操作系统则首先将磁盘上的数据拷贝到内核态的空间中, 主要是基于DMA, 也就是说这一步是不占用CPU. 然后将数据从内核空间拷贝到用户空间.

接下来, write系统调用再把数据拷贝到内核的网络发送缓冲区中, 直到内容通过网卡发送到网络中完成整个过程, 这以过程也是基于DMA.

那么这个过程中总共发生了4次数据拷贝, 2次是外部设备和中心部件的拷贝, 2次是用户态和内核态的拷贝. 即使是使用DMA来处理与外部设备的通讯, CPU仍然需要进行两次拷贝, 这一过程也发生了用户态和内核态之间的上下文切换, 加重了CPU的负担.

## 零拷贝

零拷贝的主要任务就是避免CPU将数据从一块存储拷贝到另外一块存储, 从而让CPU解放出来处理其他任务.

## sendfile()

```c
#include <sys/sendfile.h>
ssize_t sendfile(int out_fd, int in_fd, off_t* offset, size_t count);
```

系统调用sendfile()表示在in_fd和out_fd之间传输文件内容. out_fd必须指向一个套接字, in_fd指向的必须是可以mmap的. 这也表示了sendfile只能是从文件到套接字.

事实上, 当数据从外存到内核空间后, 我们是将这块缓冲区的描述符和数据长度传给socket缓冲区. 这个过程中只有DMA来完成数据拷贝, CPU并不负责, 并且避免了用户态和内核态之间的切换.
