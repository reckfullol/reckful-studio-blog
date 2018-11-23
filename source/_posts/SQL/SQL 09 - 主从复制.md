---
title: SQL 09 - 主从复制
date: 2018-08-27 22:28:32
categories: SQL
---
# 主从复制

<!--more-->

## 原理

1. 数据库有个bin-log二进制文件, 记录了所有的sql语句.

2. 主从复制就是从主数据库中将bin-log二进制文件复制到从数据库, 再执行一遍.

## 过程

整个过程需要三个进程进行实现.

1. binlog输出进程: 每当有从库连接到主库的时候, 主库都会创建一个线程然后发送binlog内容到从库. 在从库里, 当复制开始的时候, 从库会创建两个线程来处理.

2. 从库I/O线程: 当START SLAVE语句在从库开始执行之后, 从库创建一个I/O进程, 该进程连接到主库并请求主库发送binlog里面的更新到从库上. 从库I/O线程读取主库的binlog输出进程发送的更新并拷贝到这些更新到本地文件, 其中包括relay log文件.

3. 从库的SQL线程: 从库创建一个SQL线程, 这个线程读取从库I/O线程写到relay log的更新事件并执行.

![master_slave_1](https://res.cloudinary.com/dpe4i978o/image/upload/v1535379778/sql/master_slave_1.png)

![master_slave_2](https://res.cloudinary.com/dpe4i978o/image/upload/v1535379760/sql/master_slave_2.png)

## 作用

1. 做数据的热备份. 作为后备数据库, 主数据库故障后, 可以切换到从数据库继续工作, 避免数据丢失.

2. 架构的扩展. 业务量越大, I/O访问频率越高, 单机无法满足, 此时做多库的存储, 降低磁盘的I/O访问的频率, 提高单个机器的I/O性能.

3. 读写分离. 使数据库支持更大的并发, 在报表中尤其重要. 由于部分报表sql语句非常的慢, 导致锁表, 影响前台服务. 如果实现了读写分离, 前台使用master, 报表使用slave, 那么报表sql将不会导致前台锁, 保证了前台速度.