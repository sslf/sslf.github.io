---
title: Git Bash 中文乱码
date: 2018-9-3
category: 工具
tags: [git]
id: 41c9072e-e270-54d2-9a58-88512f1ac552
---


#### git status 乱码
执行：git status 命令时，本来该显示中文的地方，都显示成了数字，特别难受。 
![2018-09-03-19-43-15](http://qiniu.blog.sslfer.com/2018-09-03-19-43-15.png) 

解决办法如下：  
执行：**git config \-\-global core.quotepath false** 就恢复正常了

![2018-09-03-19-46-25](http://qiniu.blog.sslfer.com/2018-09-03-19-46-25.png)

#### git log 乱码
```shell
git config --global i18n.commitencoding utf-8
git config --global i18n.logoutputencoding utf-8
export LESSCHARSET=utf-8
```