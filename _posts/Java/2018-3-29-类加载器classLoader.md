---
layout: post
title: 类加载器classLoader
category: Java
#permalink: /java/base/classLoader.html
---

[参考](https://blog.csdn.net/briblue/article/details/54973413)

### 什么是类加载器
顾名思义就是加载类文件用的，也就是加载源代码文件编译后的class文件。只有加载到了jvm中才可以
执行。

### 加载器的分类
BootstrapClassLoader、ExtentionClassLoader、AppClassLoader、自定义的ClassLoader

### 各个加载器的关系
默认关系是：  
BootstrapClassLoader 是 ExtentionClassLoader 的父加载器。  
ExtentionClassLoader 是 AppClassLoader 的父加载器。  
AppClassLoader 是 自定义的ClassLoader 的加载器。

注意：在java中，如果获取到的父加载器是null，那么就说明实际上是BootstrapClassLoader。
所以，当我们要指明父加载器是BootstrapClassLoader的话，就直接设置为null即可。

### 加载类的过程（双亲委派）
![类加载过程](http://ozsqtghjh.bkt.clouddn.com/7548088943b82ee61f06fbcdd1ad1cf1.png)

总结：加载类是一层一层往上加载，如果找到了就一层层返回。如果找到顶层都还没有找到，那顶层加载器就开始加载。如果没有加载到，就调用下一层，一层层加载。直到最后一层加载，如果加载到就返回。没有就抛出ClassNotFindExcepton。

### ClassLoader 中重要的方法
1. loadClass （加载class的执行步骤的定义）
2. findLoadedClass （查找加载过的class）
3. findClass （查找class 文件）
4. defineClass （class文件转换为类）

### 自定义ClassLoader的使用场景
1. 自定义加载不同地方的类
2. 类的解密加载器
