---
title: Operating System 09 - 进程状态的切换
date: 2018-08-25 04:45:13
categories: Operating System
---
# 进程状态的切换

<!--more-->

![process_status_trans](https://res.cloudinary.com/dpe4i978o/image/upload/v1535143449/os/process_status_trans.png)

- 就绪状态(ready): 等待被调度
- 运行状态(running): 正在被调度
- 阻塞状态(waiting): 等待资源

## 注意

1. 只有就绪态和运行态可以相互转换, 其他都是单向转换. 就绪状态的进程通过调度算法从而获得CPU时间, 转化为运行状态. 而运行状态的进程, 在分配给他的CPU时间片用完之后就会转为就绪状态, 等待下一次调度.

2. 阻塞状态是缺少需要的资源从而由运行状态转换而来, 但是该资源不包括CPU时间, 缺少CPU时间就会从运行状态转换为就绪态.