---
title: JavaEE基础知识学习-----Apache Commons学习笔记
tags:
  - Java
originContent: ''
categories:
  - Java
toc: true
abbrlink: 63379
date: 2019-03-21 22:25:49
---

# 一、Apache Commons常用类库

## 1.1.简介

Apache Commons包含了很多开源的工具类，用于减少代码的的重复性，其中共包含类以下42个包，如下所示：
<!-- more -->
| 包名            | 说明                   | 常用类                                  |
| :-------------- | :--------------------- | --------------------------------------- |
| `BCEL`          |                        |                                         |
| `BeanUtils`     |                        |                                         |
| `BSF`           |                        |                                         |
| `Chain`         |                        |                                         |
| `CLI`           |                        |                                         |
| `Codec`         |                        |                                         |
| `Collections`   |                        | `CollectionUtils`                       |
| `Compress`      |                        |                                         |
| `Configuration` |                        |                                         |
| `Crypto`        |                        |                                         |
| `CSV`           |                        |                                         |
| `Daemon`        |                        |                                         |
| `DBCP`          |                        |                                         |
| `DbUtils`       |                        |                                         |
| `Digester`      |                        |                                         |
| `Email`         |                        |                                         |
| `Exec`          |                        |                                         |
| `FileUpload`    |                        |                                         |
| `Functor`       |                        |                                         |
| `Geometry`      |                        |                                         |
| `Imaging`       |                        |                                         |
| `IO`            | 是一个 I/O 工具集合    | `FilenameUtils`、`IOUtils`、`FileUtils` |
| `JCI`           |                        |                                         |
| `JCS`           |                        |                                         |
| `Jelly`         |                        |                                         |
| `Jexl`          |                        |                                         |
| `JXPath`        |                        |                                         |
| `Lang`          | 提供了很多通用的工具类 | `StringUtils`、`ArrayUtils`、           |
| `Logging`       |                        |                                         |
| `Math`          |                        |                                         |
| `Net`           |                        |                                         |
| `Numbers`       |                        |                                         |
| `OGNL`          |                        |                                         |
| `Pool`          |                        |                                         |
| `Proxy`         |                        |                                         |
| `RDF`           |                        |                                         |
| `RNG`           |                        |                                         |
| `SCXML`         |                        |                                         |
| `Validator`     |                        |                                         |
| `VFS`           |                        |                                         |
| `Weaver`        |                        |                                         |



# 二、Commons lang包下的常用类

## 2.1.StringUtilsl类

**字符串判空**
