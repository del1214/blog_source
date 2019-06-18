---
title: 浅谈
date: 2019-06-18 13:11:30
categories:
  - Docker
tags:
  - Docker
---

## docker 浅谈

### 这是我写docker的第一篇文章，有不对的地方请指正

我不会去讲如何在你的对应环境中安装什么，连这个都搞不定就别看了

### 什么是docker

我们所讲的docker 是由Docker官方命名为Docker Engine的软件。这款软件最核心的部分就是docker daemon 和 docker CLI.

docker daemon 通过一套RESTful API 提供镜像 ( Image )、容器 ( Container )、网络 ( Network )、数据卷 ( Volume )的管理.

我们通过docker CLI的各种命令调用docker daemon 的RESTful API来实现管理使用docker

### 怎么用

我们在windows或mac中直接使用Docker Desktop就可以了，你的电脑必须支持intel hyper-v技术

低版本的系统使用Docker Toolbox，当然我系统比较新从来没用过这个

### 为什么要用

1 开发

* 无需经过运维就可以快速创建出需要的环境
* docker hub上有各种写好服务镜像，直接站在巨人的肩膀上,例如现在用的大搜车 easymock 服务

2 测试

* 可以大量的布置环境模拟各种场景
* 可以模拟真实的线上环境提前甄别线上错误

3 部署

* 节约资源，很多服务其实是跑不满一台机器的，通过集群分配资源
* 践行devOps让上线更加可靠有保障

***可以说docker是现代开发测试运维必知必会的一个环节了***

### 后面我会按docker指令的分类来稍加描述各种指令如何使用
