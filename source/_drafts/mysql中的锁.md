---
title: Mysql 中的锁
date: 2018-04-04
category: 工具
---
参考： [锁的概念](https://www.jianshu.com/p/88231c4944f7)、 [锁的分析](https://blog.csdn.net/claram/article/details/54023216)

正式介绍之前，先来捋一捋mysql中有哪些锁的名词。
1. 乐观锁
2. 悲观锁
3. 共享锁（读锁）
4. 排它锁（写锁）
5. 行锁
6. 表锁

看是有这么多的名词，其实可以把它们分成三个层级。
1. 锁的分类 --> 乐观锁、悲观锁
2. 悲观锁的实现方式 --> 共享锁、排它锁
3. 锁的作用域 --> 行锁、表锁

具体关系，请看下图：  
![mysql 锁的关系](http://ozsqtghjh.bkt.clouddn.com/9e2f8dabc0b2ac1dbe14df29c1f308b8.png)

概述：mysql的锁可以分为两类，一类是乐观锁，另一类是悲观锁。乐观锁是要自己实现的，是一种代码逻辑锁，并没有在mysql中正在的加锁。悲观锁mysql有两种实现，一种是共享锁，另一种是排它锁。

### 乐观锁
是代码层面的逻辑控制。如果用数据库控制的话，一般就是新增一个字段来当锁。
![锁的表](http://ozsqtghjh.bkt.clouddn.com/268ab6ae571ce1c4b8d0445a9762aa94.png)  
假如要修改name字段。做法如下：
```sql
-- 得到锁的值为0
select * from tableName where id = 1;

-- 修改name值
update tableName set name='李四', lock=lock+1 where id=1 and lock = 0; -- 传入上面得到的lock值，修改成功要同时修改lock的值
```
因为没有真正的锁住表。这种操作非常适合数据多查询的表。  
什么是：查询多的？  

这里就一个疑问了，如果没有修改成功，该如何从新提交更新的操作呢？  
