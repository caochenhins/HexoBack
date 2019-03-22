---
title: JavaEE基础知识学习-----P3C问题总结
tags:
  - Java
originContent: ''
categories:
  - Java
toc: true
abbrlink: 13512
date: 2019-03-22 19:42:54
---
# P3C常用问题整理

`Object的equals方法容易抛出空指针异常，应使用常量和确定有值的对象来调用equals方法，如下`
<!-- more -->
~~~java
if(str == "2" || "2".equals(str)){} // 正确写法
if(str == "2" || str.equals("2")){} // 错误写法
~~~









