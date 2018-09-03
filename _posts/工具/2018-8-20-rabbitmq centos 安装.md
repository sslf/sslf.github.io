---
layout: post
title: rabbitmq安装
category: 工具
# permalink: /other/cluster.html
---

在安装之前，我们需要准备的运行环境有：
> 1. Erlang (是一种通用的面向并发的编程语言)
> 1. socat (端口转发工具))

另外需要明确的一点是，使用Centos的版本。以下是Centos6安装  
ps.Centos7也是一样的安装步骤，只是下载的软件版本不一样


### Erlang的安装
> wget https://dl.bintray.com/rabbitmq/rpm/erlang/21/el/6/x86_64/erlang-21.0.2-1.el6.x86_64.rpm
rpm -ivh erlang-21.0.2-1.el6.x86_64.rpm

### socat的安装
> yum -y install socat

### rabbitmq的安装
> wget https://dl.bintray.com/rabbitmq/all/rabbitmq-server/3.7.7/rabbitmq-server-3.7.7-1.el6.noarch.rpm
rpm -ivh rabbitmq-server-3.7.7-1.el6.noarch.rpm

经过以上的步骤，rabbitmq应该已经是安装成功了。那接下来就是rabbitmq的配置

``` shell
# 服务的启动
service rabbitmq-server start # 启动mq服务
rabbitmq-plugins enable rabbitmq_management # 开启web管理
chkconfig rabbitmq-server on # 开启开机启动

# 端口的开启
iptables -I INPUT -p tcp -m tcp --dport 15672 -j ACCEPT  # 开启端口
iptables -I INPUT -p tcp -m tcp --dport 5672 -j ACCEPT  # 开启端口
service iptables save  # 保持规则（要不然重启之后上面的两个规则丢失）
service iptables restart  # 重启iptables服务（可以不用执行）

# 用户的添加（默认的guest只能在localhost中登录，当然也可以更改权限）
rabbitmqctl add_user admin 123456 # 添加 用户：admin 密码：123456
rabbitmqctl set_user_tags admin administrator # 给admin管理员的角色
rabbitmqctl  set_permissions -p "/" admin '.*' '.*' '.*'  # 设置用户权限(接受来自所有Host的所有操作)
```

访问你的主机：xxxx:15672 开启rabbitmq之旅吧！
