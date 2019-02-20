---
layout: post
category: 工具
uuid: b8c75212-4125-53f4-9814-34c5c2345617
#permalink: /mysql/centos-install.html
---
安装步骤如下：
```shell
1.安装mysql的源
rpm -Uvh http://dev.mysql.com/get/mysql-community-release-el7-5.noarch.rpm

2.查看可以安装的mysql
yum repolist enabled | grep "mysql.*-community.*"

3.执行安装
yum -y install mysql-community-server

4.安装成功，加入开机启动
systemctl enable mysqld

5.启动mysql
systemctl start mysqld.service

6.配置myslq
mysql_secure_installation

配置项：
    1.设置root用户密码
    2.删除匿名用户
    3.禁止root远程登录
    4.删除test数据库
    5.刷新权限

7.配置root远程登录
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '123456' WITH GRANT OPTION;

```
