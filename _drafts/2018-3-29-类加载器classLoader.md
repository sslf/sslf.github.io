---
layout: post
title: 类加载器classLoader
category: Java基础
permalink: /java/base/classLoader.html
---
### 什么是类加载器
顾名思义就是加载类文件用的，也就是加载源代码文件编译后的class文件。

### 加载class文件到哪里，为什么要加载
我们都知道，java要运行的就必须依赖JVM，如果脱离了JVM，那我们写的java代码将以为是处。所以，
class文件是加载到了JVM里。为什么要加载同时也简单的说清楚了。

### JVM类文件简单的加载过程
加载-->验证-->准备-->解析-->初始化-->使用-->卸载

### 加载器的分类

### 加载器的实现
