---
title: High Availability 01 - 高可用概念
date: 2018-12-03 10:17:59
categories: High Availability
---
# High Availability

<!--more-->

高可用(High Availability)基本上来说, 就是要让我们的计算环境做到full-time的可用性, 在设计上一般需要:

1. 对软硬件的冗余, 以消除单点故障. 任何系统都会有一个或者多个冗余系统做standby.

2. 对故障的检测和恢复. 检测故障以及用备份的节点接管故障点, 也就是failover.

3. 需要很可靠的交汇点(CrossOver), 这些是不容易冗余的节点, 比如域名解析, 负载均衡器等.

## 冗余节点的问题

冗余节点最大的难题就是对于有状态的节点的数据复制和数据一致性的保证(无状态节点的冗余相对比较简单):

1. 如果系统的数据镜像到冗余节点是异步的, 那么在failover的时候就会出现数据差异的情况.

2. 如果系统的数据镜像到冗余节点是同步的, 那么就会导致冗余节点越多性能越慢.

## 高可用设计原理

1. 要做到数据不丢失, 就必需要持久化.

2. 要做到服务高可用, 就必需要有备用, 无论是应用节点还是数据节点.

3. 要做到复制, 就会有数据一致性的问题.

4. 我们不可能做到100%的高可用, 能做到几个9的SLA.

![ha-infrastructure](https://res.cloudinary.com/dpe4i978o/image/upload/v1543804621/high%20availability/ha-infrastructure.gif)

## 测量SLA

1. 故障发生到恢复的时间.

2. 两次故障间的时间.

大多数都是采用第一种方法, 也就是服务不可用的时间.

## 影响因素

1. 无计划宕机

    ![unplanned_downtime](https://res.cloudinary.com/dpe4i978o/image/upload/v1543805325/high%20availability/unplanned_downtime.gif)

    1. 系统级故障 - 包括主机, 操作系统, 中间件, 数据库, 网络, 电源以及外围设备等.


    2. 数据和中介的故障- 包括人员误操作, 硬盘故障等.

    3. 自然灾害, 人为破坏, 供电问题等.

2. 有计划宕机

    ![planned_downtime](https://res.cloudinary.com/dpe4i978o/image/upload/v1543805396/high%20availability/planned_downtime.gif)

    1. 日常任务: 备份, 容量规划, 用户和安全管理, 后台批处理应用等.
    
    2. 运维相关: 数据库维护, 应用维护, 中间件维护, 操作系统维护, 网络维护等.

    3. 升级相关: 数据库, 应用, 中间件, 操作系统, 网络, 包括网络升级等.