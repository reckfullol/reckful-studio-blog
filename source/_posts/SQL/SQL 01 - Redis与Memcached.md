---
title: SQL 01 - Redis与Memcached
date: 2018-08-05 15:33:53
categories: SQL
---
# Redis与Memcached

<!--more-->

两者都是KV型的非关系型数据库, 主要有以下不同:

1. 数据类型
   
   Memcached仅支持字符串类型, Redis支持五种数据类型:
   
   1. String(可存储字符串, 整数或者浮点数)
   2. List(列表)
   3. Set(无序集合)
   4. Hash(包含键值对的无序Hash表)
   5. Zest(有序集合)

2. 数据持久化
    
    Redis支持RDB快照和AOF日志两种持久化策略, Memcached不支持持久化.

3. 分布式

    Memcached不支持分布式, 只能通过在客户端使用一致性哈性来实现分布式存储, 这种方式在查询和存储的时候都需要先在客户端计算一次数据所在的节点.

    Redis Cluster实现了分布式的支持.

4. 内存管理机制

    在Redis中, 并不是所有数据一直存储在内存中的, 可以将一些很久没用的value交换到磁盘, 而Memcached会一直在内存中.

    Memcached将内存分割成特定长度的块来存储数据, 来解决内存碎片的问题. 但是会导致内存利用率不高.