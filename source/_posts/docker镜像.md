---
title: 镜像
date: 2019-06-18 13:50:53
categories:
  - Docker
tags:
  - Docker
---

在 Docker 体系里，有四个对象 ( Object ) ，几乎所有 Docker 以及周边生态的功能，都是围绕着它们所展开的。它们分别是：镜像 ( Image )、容器 ( Container )、网络 ( Network )、数据卷 ( Volume )

## 镜像 ( Image )

所谓镜像，可以理解为一个 ***只读的*** 文件包，其中包含了 ***虚拟环境运行最原始文件系统的内容***

>每次对镜像内容的修改，Docker 都会将这些修改铸造成一个镜像层，而一个镜像其实就是由其下层所有的镜像层所组成的。当然，每一个镜像层单独拿出来，与它之下的镜像层都可以组成一个镜像。
>>另外，由于这种结构，Docker 的镜像实质上是无法被修改的，因为所有对镜像的修改只会产生新的镜像，而不是更新原有的镜像。

### 镜像缓存

利用上面每一层不可修改的特性，每次的修改会产生缓存层，在制作自己的镜像时可以加速镜像的构建

例如: 精油官网的Dockerfile

```Dockerfile
FROM node:8.16.0-alpine

# 设定node-sass源
ENV SASS_BINARY_SITE https://npm.taobao.org/mirrors/node-sass
# 设定cnpm源 全局安装pm2 gulp
RUN npm config set registry http://r.cnpmjs.org/ \
  && npm i -g pm2 \
  && npm i -g gulp
RUN mkdir -p /code

COPY ./package.json /code
COPY ./npm-shrinkwrap.json /code

# 设定工作目录
WORKDIR  /code

# 安装依赖 发布文件
RUN npm i

# 拷贝源码
COPY ./ /code

# 设定生产环境变量，在此之前设定不会安装开发依赖
ENV NODE_ENV production
CMD npm run publish\
  && npm start
```

执行编译结果

1 第一步配置了node的镜像版本

2 第二步配置了node_sass环境变量

3 第三步配置了node仓库 安装了全局依赖

4 创建了代码文件夹

5 拷贝项目依赖到代码文件夹

6 拷贝项目依赖到代码文件夹

7 设置工作目录

8 依赖安装
>可以看到只要依赖关系不变，就不用添加新的依赖包

9 拷贝代码到代码文件夹

10 设置环境变量

11 执行启动命令

```bash
Building webapp
Step 1/11 : FROM node:8.16.0-alpine
 ---> e08ba08cf75a
Step 2/11 : ENV SASS_BINARY_SITE https://npm.taobao.org/mirrors/node-sass
 ---> Using cache
 ---> 9ae0bff187ae
Step 3/11 : RUN npm config set registry http://r.cnpmjs.org/   && npm i -g pm2   && npm i -g gulp
 ---> Using cache
 ---> aded62ca7c9c
Step 4/11 : RUN mkdir -p /code
 ---> Using cache
 ---> 240959fb5df5
Step 5/11 : COPY ./package.json /code
 ---> Using cache
 ---> 0cff32374228
Step 6/11 : COPY ./npm-shrinkwrap.json /code
 ---> Using cache
 ---> 302a5dfaf730
Step 7/11 : WORKDIR  /code
 ---> Using cache
 ---> 37e654041ce1
Step 8/11 : RUN npm i
 ---> Using cache
 ---> 5503b8604ed0
Step 9/11 : COPY ./ /code
 ---> 89508baebd0b
Step 10/11 : ENV NODE_ENV production
 ---> Running in 246d370b0294
Removing intermediate container 246d370b0294
 ---> c98a8f5f942c
Step 11/11 : CMD npm run publish  && npm start
 ---> Running in a10bcd80fab6
Removing intermediate container a10bcd80fab6
 ---> fbd81bfea9a3
Successfully built fbd81bfea9a3
Successfully tagged inessence_official_webapp:latest
Creating inessence_official_webapp_1 ... done
```
