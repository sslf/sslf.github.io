---
title: Mysql 事务隔离
date: 2018-9-5
category: 技术
tags: [mysql, spring]
id: f8e531b2-84c9-5e3e-96ff-caf779eeeba0
---

### 一、数据库事务的基本特性

事物是区分文件存储系统和Nosql数据库的重要特征之一。它存在的意义是保证在并发情况下的curd的正确性。那怎样才能算是正确的呢？ 
那就是事物的4个特性，即ACID：
1. A：原子性（atomicty）
    > 事物中的各个操作，要么全部成功，要么全部失败。
1. C：一致性（consistency）
    > 事物结束后，各个状态应该是一致的，是正确的。
1. I：隔离性（isolation）
    > 并发中的各个事物之间，彼此不能看到对方的中间状态。换句话说，不能查询到其他事物的修改。
1. D：持久性（durability）
    > 保存事物的所有修改

在高并发下是很难完全同时保证ACID的。除非把所有的事物都串行执行，但串行所带来的效率问题又是我们不愿意接受的。为了解决这个问题。数据库提出了
事物的隔离级别。它允许我们在不同的业务场景下使用不同的策略来做ACID和效率之间的权衡。

事物的隔离级别分为以下4种：

1. 读未提交 RU （Read uncommitted）：可以读到其他事务没有commit的数据。
1. 读提交 RC （Read committed）：只能读到其他事务已经提交了的数据，但是自己事务中的数据，可以被其他事务修改。
1. 可重复读 RR （Repeatable read）：
1. 可串行（Serializable）：所有事务依次执行，效率最低，保证ACID。

| 隔离级别 | 脏读 | 不可重复读 | 幻读 |
|---------|------|-------|------|
|未提交读  |  可能| 可能  | 可能  |
|提交读    |不可能| 可能 | 可能  |
|可重复读  |不可能| 不可能 | 可能 |
|可串行化  | 不可能| 不可能 | 不可能 |

有了上面的定义和理论。我们就来实践一下具体隔离级别的。在实践之前，先准备好实践环境。

首先创建一张测试表  
```sql
CREATE TABLE `test` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(255) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=5 DEFAULT CHARSET=utf8;
```

然后打开两个console。并且设置自动提交为off。

![2018-09-06-09-41-46](http://qiniu.blog.sslfer.com/2018-09-06-09-41-46.png)
![2018-09-06-09-42-05](http://qiniu.blog.sslfer.com/2018-09-06-09-42-05.png)

到此，环境已经准备完毕，开始测试。

#### 读未提交（RU）

先设置session的事务隔离级别为：read uncommitted（RU）。**两个console都要设置**。
```sql
set session transaction isolation level read uncommitted;
```

测试流程如下： 
![2018-09-06-10-00-54](http://qiniu.blog.sslfer.com/2018-09-06-10-00-54.png)

可以看到当前事务读取到了其他事务未提交的修改（未确认 commit）。我们把没有commit的数据称为脏数据，因为没有commit的数据还可能发生rollback。

#### 读提交（RC）

先设置session的事务隔离级别为：read committed（RC）。**两个console都要设置**。
```sql
set session transaction isolation level read committed;
```

测试流程如下： 
![2018-09-06-13-50-33](http://qiniu.blog.sslfer.com/2018-09-06-13-50-33.png)

RC级别，不能读取其他事务未提交的数据，脏读问题也就解决了，但是可以发现，在同一次事务中多次查询是数据结果不一致。这也就导致不可重复读取的问题。
如果要解决这个问题，那也就只能是RR级别了。另外，RC级别是Oracle的默认级别（未验证）。

#### 可重复读（RR）

先设置session的事务隔离级别为：repeatable read（RR）。**两个console都要设置**。
```sql
set session transaction isolation level repeatable read;
```

测试流程如下：
![2018-09-06-14-09-52](http://qiniu.blog.sslfer.com/2018-09-06-14-09-52.png)

在这个级别中。可以发现，已经解决了在同一个事务中多次查询不一致的问题。但是幻读的问题还是存在，**myslq innodb 默认的事务隔离级别是：RR，但它自己解决了幻读**。
那什么是幻读呢？

幻读，简单讲就是在事务中前后读取的数据条数不一致。感觉就像是产生幻觉一样。那它和不可重复读有什么区别呢？ 
不可重复读：是数据的修改，即update、delete。
幻读：也是数据新增，即insert。

举个例子：目前表中余额剩余1000块的人有10人。事务A查询余额为1000元的用户查询到了10人，未提交事务。之后，事务B向表中插入了一条余额也为1000的记录。
事务A再次查询余额为1000的人，结果查询出了11条记录。这就是幻读。

#### 串行
所以事务串行执行，事务隔离级别最高。效率最低。


参考：https://blog.csdn.net/a837199685/article/details/54563740