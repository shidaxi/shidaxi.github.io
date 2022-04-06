---
title: DevOps Interview Questions
date: "2021-11-13T00:00:00+08:00"
cover: "cover.png"
tags: 
  - devops
  - interview
  - cheatsheet
keywords: 
  - devops
  - interview
  - cheatsheet
description: ""
showFullContent: false
readingTime: false
---

## kubernetes
* 如何保证滚动更新过程中 0当机，从k8s, 容器，应用本身
  * 如何保证终止信号能正确传递给容器内的应用进程
*
* resource limit存在的意义是什么，cpu 内存用超了分别会发生什么；查看node已经分配了多少资源，查看实际使用率
* 自动扩容
* pod，处于pending状态/一直重启 如何排查问题
* RBAC

## cloud

### iam
* IAM role, assume role 授权机制, policy举例

### CDN
* cdn的缓存策略如何设置的

## Iac
* terraform  state文件的作用是什么
* 如何减少重复的代码，如何编写一个module
* 如何导入一个已存在的资源
* 如果runtime的资源被人为修改了或者删除了，跑terraform会发生什么，怎么处理

## monitoring
* grafana dashboard 有许多复杂的查询语句，怎么优化提高速度，recording rule 作用
* 一个counter类型指标，怎么查看它的波动情况
* 是否关注过p99延迟，p99是什么意思，如何获取指标的p99
* 指标 label是主机名，怎么查询看主机名包含 某个关键词的指标

## logging
* elasticsearch 优化

## cicd
* 构建 部署，如何从技术上提高构建效率和部署效率
* 蓝绿，金丝雀 区别与实现方式

## Java
* 接触过的jvm参数及含义
* 内存模型 jdk8/jdk11,
* java/springboot 如何监控，关注的监控指标有哪些
* 排错
  * 遇到过哪些java层面问题，如何排查的
  * java服务响应慢，从jvm层面怎么排查
  * 有哪些排错手段

## 编程
* 用python写过哪些

## 工作方法
* 从部门协作的角度，devops如何提高我们的支持效率？
  * 时间主要花费在哪里？ 如何减少不必要的时间开销？

## 未涉及
* 还有哪些地方比较擅长，但是没有问到的