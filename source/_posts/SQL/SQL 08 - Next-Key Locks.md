---
title: SQL 08 - Next-Key Locks
date: 2018-08-26 03:13:24
categories: SQL
---
# Next-Key Locks

<!--more-->

Next-Key Locks是MySQL的InnoDB存储引擎的一种锁实现.

MVCC不能解决幻读问题, Next-Key Locks就是为了解决这个问题而存在的. 在可重复读隔离级别, 用MVCC+Next-Key Locks可以解决幻读问题.

## Record Locks

锁定一个记录上的索引, 而不是记录本身, 如果表没有设置索引, InnoDB会在主键上创建隐藏的聚簇索引, 因此Record Locks依然可以使用.

## Gap Locks

锁定索引之间的间隙, 但是不包含索引本身. 例如当一个事务执行以下语句, 其他事务就不能在t.c中插入15.

```sql
SELECT c FROM t WHERE c BETWEEN 10 AND 20 FOR UPDATE;
```

## Next-Key Locks

他是Record Locks和Gap Locks的结合, 不仅锁定一个记录上的索引, 也锁定索引之间的间隙. 例如一个索引包含以下值, 10, 11, 13, 20, 那么需要锁定以下区间.

```
(negative infinity, 10]
(10, 11]
(11, 13]
(13, 20]
(20, positive infinity)
```