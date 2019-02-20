---
title: Centos7 修改源镜像
date: 2018-3-31
category: 工具
id: 675c6c40-5fd5-5f63-8af9-e5fedf20a22a
---
默认的源在国内访问一点慢。修改为阿里云的

```shell
先安装 wget
yum install wget -y

备份当前的yum源
mv /etc/yum.repos.d /etc/yum.repos.d.backup4comex

新建空的yum源设置目录
mkdir /etc/yum.repos.d

下载安装阿里云的yum源配置
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo

情况仓库缓存
yum clean all

重新创建缓存
yum makecache

大功告成
```
