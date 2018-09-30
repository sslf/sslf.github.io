---
layout: post
category: 工具
tags: [git]
---

执行：git status 命令时，本来该显示中文的地方，都显示成了数字，特别难受。 
![2018-09-03-19-43-15](http://ozsqtghjh.bkt.clouddn.com/2018-09-03-19-43-15.png) 

解决办法如下：  
执行：**git config \-\-global core.quotepath false** 就恢复正常了

![2018-09-03-19-46-25](http://ozsqtghjh.bkt.clouddn.com/2018-09-03-19-46-25.png)