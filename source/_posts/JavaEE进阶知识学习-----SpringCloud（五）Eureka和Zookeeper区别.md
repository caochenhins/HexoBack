---
title: JavaEE进阶知识学习-----SpringCloud（五）Eureka和Zookeeper区别
tags:
  - SpringCloud
originContent: ''
categories:
  - SpringCloud
toc: true
abbrlink: 13512
date: 2019-03-23 05:44:04
---
# Eureka和Zookeeper区别

## 遵循原则不同

**Eureka遵循AP原则，Zookeeper遵循CP原则**，C：强一致性，A：可用性，P：分区容错性

著名的CAP理论中提出，一个分布式系统不可能同时满足C(一致性)A(可用性)P(分区容错性)，由于分区容错性p是分布式系统中必须保证，因此只能在A和C之间权衡
<!-- more -->
## Zookeeper保证CP

在Zookeeper中存在一种情况下，当master节点因为网络故障与其他节点失去联系时，剩余节点会重新进行leader选举，但是，选举leader的时间太长，且选举过程中这个Zookeeper集群是不可用的，这就导致在选举期间注册服务瘫痪，在云部署的环境中，因为网络问题使得Zookeeper集群失去master节点的可能性较大，虽然服务最终能够恢复，但是在漫长的选举时间导致的注册时间不可用是不能容忍的，当我们向注册中心查询注册列表时，可以忍受注册中心返回的是几分钟以前的注册信息，但是不能接收服务直接down不可用，也就是说，服务注册对可用性的要求高于一致性。

## Eureka保证AP

Eureka知道Zookeeper的不足，所以设计最初就保证可用性，Eureka各个节点都是平等的，几个节点的挂点不会影响其他正常节点的工作，剩余的节点仍然可以提供注册和查询服务，只不过不能保证查询的信息是最新的，除此之外，Eureka还有一种自我保护机制，当过多的节点没有正常的心跳时，那么Eureka就会认为客户端出现了网络故障，此时Eureka会

- Eureka不会从注册表中移除因为长时间没有收到心跳而应该过期的服务
- Eureka仍然能够接受新服务的注册和查询请求，但是不会同步到其他节点上（保证当前节点可用）
- 当网络稳定时，当前实例新的注册信息会被同步到其他节点上