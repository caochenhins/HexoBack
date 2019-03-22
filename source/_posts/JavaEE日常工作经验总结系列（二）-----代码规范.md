---
title: JavaEE日常工作经验总结系列（二）-----代码规范
tags:
  - Java
originContent: ''
categories:
  - Java
toc: true
abbrlink: 13512
date: 2019-03-22 19:42:55
---
# 2018.06.15代码规范总结

* 控制层的方法体尽可能小，数据处理代码放在业务处理层
* 拒绝代码中出现魔法数字，使用常量的形式
* 字符串使用StringUtils工具类判空
* 集合list使用CollectionUtils工具类进行判空
* 遍历Map使用entrySet，避免使用keySet
* 注意代码的缩进格式，是否进行的格式化
<!-- more -->