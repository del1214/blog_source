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

这个指令一般都出现在Dockerfile的第一行，接下来的所有工作都是基于`FROM`指令指定的镜像开展的。一个合法的Dockerfile必须以`FROM`指令开头。

* `ARG` 指令允许在`FROM`之前出现
* `FROM` 可以在单个Dockerfile中出现多次用来构建多个镜像或作为其他镜像的依赖。提交(`commit`)之前指令构建的镜像(`image`)获取ID，之后的`FROM`会清除前面指令的状态
* `FROM`指令可以选择性的添加`AS name`参数来给予当前构建状态命名。命名可以被之后的`FROM`和`COPY --from=<name|index>`指令引用当前阶段的镜像(`image`)
* `tag`和`digest`是可选的，如果忽略它们，构造器会默认添加`latest`标签。若没有找到tag会报错

```Dockerfile
FROM <image> [AS <name>]
FROM <image>[:<tag>] [AS <name>]
FROM <image>[@<digest>] [AS <name>]
```

#### RUN

`RUN`指令用来执行shell命令，并且在当前镜像的最高层级上新创建一层并记录下来

`RUN`有2中格式，但我个人只使用第一种

***第二种方式会被解析成JSON数组，所以里面的命令必须用双引号包裹***

```Dockerfile
RUN <command>
RUN ["executable", "param1", "param2"]
```

##### 默认使用shell命令

`RUN`默认使用`/bin/sh` shell执行命令

>使用bash，当然有的极简镜像不包含bash...

```Dockerfile
RUN /bin/bash -c 'echo hello'
```

##### 使用换行符

如果你的命令过长不易读可以使用换行符`\`来多行编写

```Dockerfile
RUN /bin/bash -c \
  'source $HOME/.bashrc;
```

##### 一次执行多条语句

通常不会一次只执行一条命令而是多条可以用`;`来分割

```Dockerfile
RUN npm config set \
  registry http://r.cnpmjs.org/; \
  npm i -g pm2; \
  npm i -g gulp;
```

#### CMD

```Dockerfile
# (exec form, 推荐使用)
CMD ["executable","param1","param2"]
#  (ENTRYPOINT参数)
CMD ["param1","param2"]
#  (shell form)
CMD command param1 param2
```
