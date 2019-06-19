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

### 查看镜像

```bash
docker images
REPOSITORY                                 TAG                 IMAGE ID            CREATED             SIZE
inessence_official_webapp                  latest              fbd81bfea9a3        3 hours ago         1.16GB
io_ts_webapp                               latest              666d494004eb        5 days ago          406MB
<none>                                     <none>              0476fa281a13        5 days ago          406MB
<none>                                     <none>              9b4121e2db9f        5 days ago          406MB
<none>                                     <none>              fa23efbdda7d        5 days ago          406MB
mongo                                      <none>              0a8d98121dec        5 days ago          426MB
<none>                                     <none>              b9f227d5c6f7        6 days ago          1.08GB
<none>                                     <none>              a083f63f5823        6 days ago          1.08GB
<none>                                     <none>              f75daa7736f5        6 days ago          1.08GB
<none>                                     <none>              268ac0217a08        6 days ago          226MB
<none>                                     <none>              ab24983e2216        6 days ago          1.08GB
<none>                                     <none>              92984335200c        6 days ago          1.08GB
<none>                                     <none>              f99fee163ae5        6 days ago          1.08GB
<none>                                     <none>              e6fbaa07088f        6 days ago          1.08GB
<none>                                     <none>              8a2c102b92c5        6 days ago          1.08GB
nginx                                      latest              719cd2e3ed04        7 days ago          109MB
zabbix/zabbix-server-mysql                 latest              890289d63b00        7 days ago          65.3MB
zabbix/zabbix-web-apache-mysql             latest              abbb7efb4887        7 days ago          108MB
zabbix/zabbix-agent                        latest              4b2ee6b413bc        7 days ago          14.9MB
node                                       8.16.0-alpine       e08ba08cf75a        13 days ago         66.7MB
sinopia-docker_sinopia                     latest              a9a2ab793aa0        2 weeks ago         143MB
sinopia                                    latest              ddf619bf46b6        2 weeks ago         143MB
node                                       alpine              91acf04599c4        3 weeks ago         79.6MB
mongo                                      3.4                 3e07e22f0dbf        4 weeks ago         426MB
mysql                                      latest              990386cbd5c0        5 weeks ago         443MB
easymock/easymock                          1.6.0               193a7b904d4f        5 weeks ago         699MB
mysql                                      5.7                 7faa3c53e6d6        5 weeks ago         373MB
k8s.gcr.io/kubernetes-dashboard-amd64      v1.10.1             f9aed6605b81        6 months ago        122MB
k8s.gcr.io/kube-proxy-amd64                v1.10.11            7387003276ac        6 months ago        98.3MB
k8s.gcr.io/kube-apiserver-amd64            v1.10.11            e851a7aeb6e8        6 months ago        228MB
k8s.gcr.io/kube-controller-manager-amd64   v1.10.11            978cfa2028bf        6 months ago        151MB
k8s.gcr.io/kube-scheduler-amd64            v1.10.11            d2c751d562c6        6 months ago        51.2MB
docker/kube-compose-controller             v0.4.12             02a45592fbea        9 months ago        27.8MB
docker/kube-compose-api-server             v0.4.12             0f92c77fa676        9 months ago        41.2MB
k8s.gcr.io/etcd-amd64                      3.1.12              52920ad46f5b        15 months ago       193MB
mritd/idgen                                latest              07e1eab12bef        15 months ago       35.7MB
k8s.gcr.io/k8s-dns-dnsmasq-nanny-amd64     1.14.8              c2ce1ffb51ed        17 months ago       41MB
k8s.gcr.io/k8s-dns-sidecar-amd64           1.14.8              6f7f2dc7fab5        17 months ago       42.2MB
k8s.gcr.io/k8s-dns-kube-dns-amd64          1.14.8              80cc5ea4b547        17 months ago       50.5MB
k8s.gcr.io/pause-amd64                     3.1                 da86e6ba6ca1        18 months ago       742kB
k8s.gcr.io/pause                           3.1                 da86e6ba6ca1        18 months ago       742kB
redis                                      4.0.6               1e70071f4af4        18 months ago       107MB
quay.io/coreos/hyperkube                   v1.7.6_coreos.0     2faf6f7a322f        21 months ago       699MB
skyzhou/docker-discuz                      latest              54339c48016c        4 years ago         269MB
```

### 获取镜像

```bash
docker pull node
Using default tag: latest
latest: Pulling from library/node
6f2f362378c5: Downloading [=======>                                           ]  6.876MB/45.34MB
494c27a8a6b8: Download complete
7596bb83081b: Download complete
372744b62d49: Downloading [=====>                                             ]   5.09MB/50.07MB
615db220d76c: Waiting
afaefeaac9ee: Waiting
22d677ae7b14: Waiting
954f64c2b02a: Waiting
3a0d282381d6: Waiting
```

### 搜索镜像

```bash
docker search node
NAME                                   DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
node                                   Node.js is a JavaScript-based platform for s…   7531                [OK]
mongo-express                          Web-based MongoDB admin interface, written w…   468                 [OK]
nodered/node-red-docker                Node-RED Docker images.                         316                                     [OK]
selenium/node-chrome                                                                   194                                     [OK]
prom/node-exporter                                                                     136                                     [OK]
iojs                                   io.js is an npm compatible platform original…   130                 [OK]
selenium/node-firefox                                                                  121                                     [OK]
circleci/node                          Node.js is a JavaScript-based platform for s…   87
readytalk/nodejs                       Node.js based off the official Debian Wheezy…   51                                      [OK]
digitallyseamless/nodejs-bower-grunt    Node.js w/ Bower & Grunt Dockerfile for tru…   48                                      [OK]
kkarczmarczyk/node-yarn                Node docker image with yarn package manager …   47                                      [OK]
bitnami/node                           Bitnami Node.js Docker Image                    38                                      [OK]
iron/node                              Tiny Node image                                 29
calico/node                                                                            17                                      [OK]
appsvc/node                            Azure App Service Node.js dockerfiles           12                                      [OK]
centos/nodejs-8-centos7                Platform for building and running Node.js 8 …   8
cusspvz/node                           🌐 Super small Node.js container (~15MB) bas…    7                                       [OK]
mc2labs/nodejs                         CoffeScript and Supervisor powered Nodejs ba…   7                                       [OK]
basi/node-exporter                     Node exporter image that allows to expose th…   7                                       [OK]
centos/nodejs-6-centos7                Platform for building and running Node.js 6 …   4
ppc64le/node                           Node.js is a JavaScript-based platform for s…   2
nodecg/nodecg                          Create broadcast graphics using Node.js and …   1                                       [OK]
ogazitt/node-env                       node app that shows environment variables       0
camptocamp/node-collectd               rancher node monitoring agent                   0                                       [OK]
appsvctest/node                        node build                                      0                                       [OK]
```

当然上面的太极客了，我一般是去[Docker Hub](https://hub.docker.com/)搜索

### 使用阿里云仓库

linux 下

```bash
sudo vim /etc/docker/daemon.json

# 写入

# {

# "registry-mirrors": ["https://dftbcros.mirror.aliyuncs.com"]

# }

systemctl restart docker即可
```

Docker Desktop直接在首选项的Daemon面板添加Registry mirrors项即可

### 从容器创建一个新的镜像

```bash
docker commit -a "runoob.com" -m "my apache" a404c6c174a2  mymysql:v1
docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]

```

### 标记镜像

```bash
docker tag [OPTIONS] IMAGE[:TAG] [REGISTRYHOST/][USERNAME/]NAME[:TAG]
```

### 保存镜像

将指定镜像保存成tar包

```bash
docker save -o my_node_v1.tar node
docker save node > my_node_v1.tar
```

### 导入镜像

只是导入

```bash
docker load -i my_node_v1.tar
docker load < my_node_v1.tar
```

### 从压缩包创建镜像

```bash
docker import my_node_v1.tar my_node_v1
REPOSITORY                                 TAG                 IMAGE ID            CREATED             SIZE
my_node_v1                                 latest              39dba8b60aa0        6 seconds ago       1.08GB
```

### 查看镜像创建历史

```bash
docker history my_node_v
```

### 查看镜像元数据

```bash
docker inspect IMAGE
```

### 删除镜像

通俗易懂，牛逼的在下面

```bash
docker rmi IMAGE
```

### 构建镜像

参见Dockerfile部分

### 实用技巧

#### 删除untagged镜像

构建镜像时产生的中间层，那些id为 <None>的image的话可以用

```bash
docker rmi $(docker images | grep "^<none>" | awk "{print $3}")
```

#### 删掉全部镜像

```bash
docker rmi $(docker images -q)
```

#### 镜像缓存

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

```bash
Building webapp
Step 1/11 : FROM node:8.16.0-alpine
# 第一步配置了node的镜像版本
 ---> e08ba08cf75a
Step 2/11 : ENV SASS_BINARY_SITE https://npm.taobao.org/mirrors/node-sass
# 第二步配置了node_sass环境变量
 ---> Using cache
 ---> 9ae0bff187ae
Step 3/11 : RUN npm config set registry http://r.cnpmjs.org/   && npm i -g pm2   && npm i -g gulp
# 第三步配置了node仓库 安装了全局依赖
 ---> Using cache
 ---> aded62ca7c9c
Step 4/11 : RUN mkdir -p /code
# 创建了代码文件夹
 ---> Using cache
 ---> 240959fb5df5
Step 5/11 : COPY ./package.json /code
# 拷贝项目依赖到代码文件夹
 ---> Using cache
 ---> 0cff32374228
Step 6/11 : COPY ./npm-shrinkwrap.json /code
# 拷贝项目依赖到代码文件夹
 ---> Using cache
 ---> 302a5dfaf730
Step 7/11 : WORKDIR  /code
# 设置工作目录
 ---> Using cache
 ---> 37e654041ce1
Step 8/11 : RUN npm i
# 依赖安装
# >可以看到只要依赖关系不变，就不用添加新的依赖包
 ---> Using cache
 ---> 5503b8604ed0
Step 9/11 : COPY ./ /code
# 拷贝代码到代码文件夹
 ---> 89508baebd0b
Step 10/11 : ENV NODE_ENV production
# 设置环境变量
 ---> Running in 246d370b0294
Removing intermediate container 246d370b0294
 ---> c98a8f5f942c
Step 11/11 : CMD npm run publish  && npm start
# 执行启动命令
 ---> Running in a10bcd80fab6
Removing intermediate container a10bcd80fab6
 ---> fbd81bfea9a3
Successfully built fbd81bfea9a3
Successfully tagged inessence_official_webapp:latest
Creating inessence_official_webapp_1 ... done
```
