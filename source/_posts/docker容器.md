---
title: 容器
date: 2019-06-18 14:19:07
categories:
  - Docker
tags:
  - Docker
---

## 容器 ( Container )

容器就是被隔离出来的虚拟环境

容器的生命周期: ***Created,Running,Paused,Stopped,Deleted***

Running是真正在运行的容器

保持一个容器处在Running状态就需要容器中PID为1的进程一直运行着，一旦这个进程停止意味着容器的停止

>容器启动进程往往由Dockerfile中最后的ENTRYPOINT或CMD决定
>也就是这2选1个参数执行的shell脚本执行的程序不能报错，且必须停留在命令行上，

### 创建容器

```bash
docker create --name mysql -e MYSQL_ROOT_PASSWORD=123456 mysql:latest
8101a3998e3c86fe266e3ec35db21263ab645c982043daa84e3b985445ebf5fc
```

执行 `docker create` 后，Docker 会根据命令使用镜像创建容器，并在控制台打印出容器 ID，此时容器是处于 Created 状态的。

我们可以用容器ID或容器name来操作容器

### 启动容器

```bash
docker start mysql
```

通过 `docker start`命令启动了容器

有人会说真麻烦，创建一步启动一步，下面就一键启动

>看了docker start的参数发现它可以一次启动多个容器，也并非一无是处

```bash
docker start [OPTIONS] CONTAINER [CONTAINER...]
```

#### docker run

`docker run`将`docker create`和`docker start`两步并作一步，容器创建后会立刻启动

`docker run` 是我们居家旅行杀人灭口必备命令它能干的事非常多

##### ***高级内容，不熟可跳过***

```bash
docker run [OPTIONS] IMAGE:TAG [COMMAND] [ARG...]

# 常用参数
# -a stdin: 指定标准输入输出内容类型，可选 STDIN/STDOUT/STDERR 三项；

# -d: 后台运行容器，并返回容器ID；

# -i: 以交互模式运行容器，通常与 -t 同时使用；

# -P: 随机端口映射，容器内部端口随机映射到主机的高端口

# -p: 指定端口映射，格式为：主机(宿主)端口:容器端口

# -t: 为容器重新分配一个伪输入终端，通常与 -i 同时使用；

# --name="nginx-lb": 为容器指定一个名称；

# --dns 8.8.8.8: 指定容器使用的DNS服务器，默认和宿主一致；

# --dns-search example.com: 指定容器DNS搜索域名，默认和宿主一致；

# -h "mars": 指定容器的hostname；

# -e username="ritchie": 设置环境变量，可指定多个；

# --env-file=[]: 从指定文件读入环境变量；

# --cpuset="0-2" or --cpuset="0,1,2": 绑定容器到指定CPU运行；

# -m :设置容器使用内存最大值；

# --net="bridge": 指定容器的网络连接类型，支持 bridge/host/none/container: 四种类型；

# --rm $container_name: 如果容器存在则删除

# --link=[]: 添加链接到另一个容器；

# --expose=[]: 开放一个端口或一组端口；
```

>***保持一个容器运行不退出的必须是 ENTRYPOINT 或 CMD 占据bash不退出的命令***

##### ***实用命令***

后台运行
>后台启动一个以nginx最新版本镜像的容器，命名为mynginx

```bash
docker run --name mynginx -d nginx:latest
```

端口绑定
>docker run 子命令启动容器，-p 将容器的 8000 端口映射到宿 主机的 8000 端口上，--name 给容器赋予一个唯一的名字， 最后一个参数是镜像

```bash
docker run -p 8000:8000 --name $container_name $image_name
```

交互方式运行
>使用-t -i用交互方式运行
>镜像名后面固定跟着的是 cmd

```bash
docker run -t -i $image_name /bin/bash
```

挂载目录
>your_dir必须是绝对路径

```bash
 docker run -v /your_dir:/target_dir debian
```

指定环境变量
>启动一个mysql

```bash
docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=123456 -d -e MYSQL_DATABASE=my_database -v /your_abs_dir:/var/lib/mysql mysql:latest
```

容器退出时删除数据卷
>
```bash
docker run --name xxx --rm mysql
```

### 管理容器

#### 查看容器列表

通过 `docker ps`可以列出Docker中的容器

```bash
docker ps
# 默认情况下只列出Running状态的容器
CONTAINER ID        IMAGE                       COMMAND                  CREATED             STATUS              PORTS                    NAMES
2884ecbd2e57        inessence_official_webapp   "docker-entrypoint.s…"   About an hour ago   Up About an hour    0.0.0.0:8888->3000/tcp   inessence_official_webapp_1
04c32cf7999c        mysql:latest                "docker-entrypoint.s…"   4 hours ago         Up 4 hours          3306/tcp, 33060/tcp      fuck
59491db0f044        mongo:3.4                   "docker-entrypoint.s…"   22 hours ago        Up 6 hours          27017/tcp                easy-mock-docker-160_mongodb_1
```

```bash
docker ps -a
docker ps --all
# 列出全部
```

通过`docker ps`可以看到容器ID(CONTAINER ID),使用镜像(IMAGE),执行命令(COMMAND),创建时间(CREATED),状态(STATUS),端口(PORTS),名称(NAMES)

其中状态(STATUS)有3种状态

* Created 此时容器已创建，但还没有被启动过
* Up [ Time ] 这时候容器处于正在运行状态，而这里的 Time 表示容器从开始运行到查看时的时间
* Exited ([ Code ]) [ Time ] 容器已经结束运行，这里的 Code 表示容器结束运行时，主程序返回的程序退出码，而 Time 则表示容器结束到查看时的时间

一般你都会看到第三种的，等你成为高手了总会是第二种，😊

### 停止容器

#### docker stop

```bash
docker stop mysql
```

容器停止后，内部被修改的内容会保留，可以通过 docker start 命令将这个容器再次启动

### 暂停恢复容器内进程

```bash
docker pause mysql
# docker pause [OPTIONS] CONTAINER [CONTAINER...]
docker unpause mysql
# docker unpause [OPTIONS] CONTAINER [CONTAINER...]
```

### 查看容器进程信息

```bash
docker top mysql
# docker top CONTAINER [ps OPTIONS]
```


### 删除容器

#### docker rm

```bash
docker rm mysql
```

运行中的容器是不允许被删除的，可以加参数

```bash
docker rm -f mysql
docker rm --force
```

### 进入容器

很多时候我们都想深入了解对方，是时候进入容器了

#### 在容器内执行命令

```bash
# docker exec [OPTIONS] CONTAINER COMMAND [ARG...]
docker exec mysql cat /etc/hostname
```

#### ***在容器内启动bash***

```bash
docker exec -it mysql /bin/bash
root@6c1988417237:/#
```

### 查看容器日志

有时我们需要观察容器在启动和运行过程中打印在bash上的日志

```bash
docker logs CONTAINER
```

### 查看容器元数据

成为高手后会经常查看吧，反正我没看过

```bash
docker inspect CONTAINER
```

### docker container

专门管理容器的命令,更细化了管理能力,我就不一一翻译了

```bash
Usage:	docker container COMMAND

Manage containers

Commands:
  attach      Attach local standard input, output, and error streams to a running container
  commit      Create a new image from a container's changes
  cp          Copy files/folders between a container and the local filesystem
  create      Create a new container
  diff        Inspect changes to files or directories on a container's filesystem
  exec        Run a command in a running container
  export      Export a container's filesystem as a tar archive
  inspect     Display detailed information on one or more containers
  kill        Kill one or more running containers
  logs        Fetch the logs of a container
  ls          List containers
  pause       Pause all processes within one or more containers
  port        List port mappings or a specific mapping for the container
  prune       Remove all stopped containers
  rename      Rename a container
  restart     Restart one or more containers
  rm          Remove one or more containers
  run         Run a command in a new container
  start       Start one or more stopped containers
  stats       Display a live stream of container(s) resource usage statistics
  stop        Stop one or more running containers
  top         Display the running processes of a container
  unpause     Unpause all processes within one or more containers
  update      Update configuration of one or more containers
  wait        Block until one or more containers stop, then print their exit codes
```

### 实用命令

#### 停止所有的容器

```bash
docker stop $(docker ps -a -q)
```

#### 删除所有容器

```bash
docker rm $(docker ps -a -q)
docker rm -f $(docker ps -a -q)
```

#### 获取容器ip

```bash
docker inspect \
> --format '{{.NetworkSettings.IPAddress}}' \
> container_id
```

#### 查看所有容器进程信息

```bash
for i in  `docker ps |grep Up|awk '{print $1}'`;do echo \ &&docker top $i; done
```

## To Be Continue
