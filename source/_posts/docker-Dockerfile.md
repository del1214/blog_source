---
title: Dockerfile
date: 2019-06-18 16:29:09
categories:
  - Docker
tags:
  - Docker
---

## Dockerfile

Dockerfile是用于构建Docker镜像的配置文件。在Dockerfile中包含了一些列构建镜像需要执行的命令操作。利用Docker镜像体积小的特点能够快速实现迁移部署。

Dockerfile就是一个名为Dockerfile的文件。

Dockerfile有自己的语法，但十分简单，只有2种：指令和注释

```Dockerfile
INSTRUCTION arguments
# 注释
```

### Dockerfile 指令

#### FROM

使用FROM指定一个基础镜像

这个指令一般都出现在Dockerfile的第一行，接下来的所有工作都是基于FROM指令指定的镜像开展的

```Dockerfile



```