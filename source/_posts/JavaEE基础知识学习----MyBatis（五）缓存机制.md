---
title: JavaEE基础知识学习----MyBatis（五）缓存机制
tags:
  - MyBatis
originContent: ''
categories:
  - MyBatis
toc: true
abbrlink: 13512
date: 2019-03-22 19:44:53
---
# MyBatis的缓存机制

## 概述

MyBatis 包含一个非常强大的查询缓存特性,它可以非常方便地配置和定制。缓存可以极大的提升查询效率。MyBatis系统中默认定义了两级缓存，一级缓存和二级缓存。

- 默认情况下，只有一级缓存（SqlSession级别的缓存，也称为本地缓存）开启。
- 二级缓存需要手动开启和配置，他是基于namespace级别的缓存。
- 为了提高扩展性。MyBatis定义了缓存接口Cache。我们可以通过实现Cache接口来自定义二级缓存
<!-- more -->
## 一级缓存（本地缓存）

与数据库同一次会话期间查询的数据会放在本地缓存中，以后如果需要获取相同数据，直接从缓存中拿，不再查询数据库

**一级缓存失效的四种情况**

一级缓存是sqlSession级别的缓存，也就说一个sqlSession拥有自己的一级缓存，一级缓存是一直开启的，没有使用一级缓存的情况

- 不同的sqlSession对应不同的一级缓存
- 同一个sqlSession,但是查询条件不一样
- 同一个sqlSession两次查询期间执行了任何一次增删改操作
- 同一个sqlSession两次查询期间手动清空了缓存

## 二级缓存（全局缓存）

- 二级缓存基于namespace默认不开启，需要手动配置
- MyBatis提供二级缓存的接口以及实现，缓存实现要求POJO实现Serializable接口
- **二级缓存在SqlSession 关闭或提交之后才会生效**

**工作机制**

- 一个会话，查询一条数据，这个数据就会放在当前会话的一级缓存中
- 如果会话关闭，一级缓存中的数据就会保存到二级缓存中，新的查询信息，就参照二级缓存

**使用步骤**

- 全局配置文件中开启二级缓存

```xml
<setting name="cacheEnabled" value="true"/>
```

- 需要使用二级缓存的映射文件mapper.xml处使用cache配置缓存

```xml
<cach></cach>
```

- 注意：POJO需要实现Serializable接口

Mybatis提供了整合ehcache缓存，具体整合方法参考官网文档，