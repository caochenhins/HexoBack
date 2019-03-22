---
title: JavaEE日常工作经验总结系列（一）-----Java8优雅去重
tags:
  - Java
originContent: ''
categories:
  - Java
toc: true
abbrlink: 13512
date: 2019-03-22 19:42:55
---
**字符串集合去重**

```java
List<String> distinctElements = list.stream().distinct().collect(Collectors.toList());
```
<!-- more -->
**根据对象属性去重**

```java
public static <T> Predicate<T> distinctByKey(Function<? super T, Object> keyExtractor)
    {
        Map<Object, Boolean> map = new ConcurrentHashMap<>();
        return t -> map.putIfAbsent(keyExtractor.apply(t), Boolean.TRUE) == null;
    }
//使用举例
persons.stream().filter(distinctByKey(Person::getName))
```

