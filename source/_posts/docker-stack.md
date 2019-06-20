---
title: docker stack
date: 2019-06-20 17:06:37
categories:
  - Docker
tags:
  - Docker
---

## docker stack

docker stack 需要在编写完 docker-compose(会读取Compose配置文件) 和创建好 docker swarm(使用Node Worker) 后使用

### ***docker stack deploy***

开启stack，更新stack
别名
> docker stack up

部署docker-compose编排服务

```bash
docker stack deploy $stack_name -c docker-compose.yml

# $stack_name stack名称
# -c docker-compose配置文件
```

### ***docker stack rm***

停止stack，删除stack
别名
> docker stack down

```bash
docker stack rm $stack_name

# $stack_name stack名称
```

### docker stack ls

显示stack列表

别名
> docker stack list

### docker stack ps

显示正在运行的stack列表

```bash
docker stack ps
# 常用参数
# -a 显示全部容器
# -f 根据条件过滤(匹配关系),例name=worker
# -l 显示刚创建的容器
# -q 只返回容器id值
# -s 显示数量
```

### docker stack services

显示stack中的服务

```bash
docker stack services $stack_name

# $stack_name stack名称

# 常用参数
# -f 过滤
# -q 返回服务id
```

## 实用技巧

### 读取日志

```bash
docker-compose ps | tail -n +3 | awk '{print $1}' | xargs -n1 docker logs
```

至此，我们已经完成了从编写Dockerfile跑webapp,再到编写docker-compose跑整个服务，再到集群化部署服务的过程。

写的比较简单，还需要遇到真实场景去做更复杂的处理
