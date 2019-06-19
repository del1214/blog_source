---
title: docker-swarm
date: 2019-06-18 16:29:42
categories:
  - Docker
tags:
  - Docker
---

## docker swarm

docker swarm 是 docker亲生的管理docker集群的工具。kubernetes是google私生的，不过现在已经转正。

### swarm

swarm是一个运行docker engine节点(node)的集合，每台跑着docker的机器都可以被视为一个节点。这个节点集合可以用来发布和编排服务。

节点可以主动初始化一个swarm或者加入一个已经存在的swarm。这样该节点就成为了swarm中的一个节点。

#### 角色

swarm中的节点分为2种角色

##### manager

manager节点用于cluster的管理，swarm命令基本只能在manager节点执行。一个cluster可以有多个manager节点，但只有一个节点可以成为leader manager node，leader选主通过raft协议实现，参数可配置。

##### worker

worker节点是任务执行节点，manager将service下发至worker节点执行。

>STATUS为空的是worker节点，Reachable的是非leader manager node。Manager节点默认也作为worker节点。

#### 检查一个节点是否处于swarm

```bash
# 处于
Swarm: active
# 不处于
Swarm: inactive
```

#### 创建集群

```bash
docker swarm init --listen-addr ip:port  --advertise-addr ip
# --listen-addr 监听ip:端口
# --advertise-addr 有多块网卡时制定端口

Swarm initialized: current node (kb7aqhna2cz9e3i3v4ewkc7sp) is now a manager.

To add a worker to this swarm, run the following command:

    docker swarm join --token SWMTKN-1-5jm4q3ern4f4u76wve3dofol2rav253of42ygxf6nk78t18re9-c35uk3qc4l2itli9rkt87qqem 19
2.168.65.3:2377

To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.
```

>创建集群的节点，自己就是manager了。会返回加入worker所需的token

#### 获取加入角色 token

```bash
docker swarm join-token manager
To add a manager to this swarm, run the following command:

    docker swarm join --token SWMTKN-1-5jm4q3ern4f4u76wve3dofol2rav253of42ygxf6nk78t18re9-cd8lmiift26osjk1cqqhpy1hy 192.168.65.3:2377
docker swarm join-token worker
To add a worker to this swarm, run the following command:

    docker swarm join --token SWMTKN-1-5jm4q3ern4f4u76wve3dofol2rav253of42ygxf6nk78t18re9-c35uk3qc4l2itli9rkt87qqem 192.168.65.3:2377
```

#### 加入集群

>在其他节点上执行上面获取的命令即可

#### 退出集群

```bash
docker swarm leave --force
# --force 强制退出
```

> 已经跑起服务的需要强制退出

#### Service与Task

Task是swarm中的最小原子调度单位，就是一个单一的容器
Service 是一组Task的集合，Service 定义 Task的属性

Service 有两种模式:
* ***replicated service***: 按一定规则在各节点上运行指定个数的Task

* ***global service***:每个节点上只运行一个task

>因为有docker stack 绝大部分指令了解即可

##### 创建服务

```bash
docker service create --replicas 2 --name helloworld --network=swarm_test nginx:alpine
```

##### 查看服务列表

```bash
docker service ls
# ID            NAME        REPLICAS  IMAGE         COMMAND
# 5gz0h2s5agh2  helloworld  0/2       nginx:alpine  
```

##### 查看运行中服务

```bash
docker service ps helloworld
# ID          NAME          IMAGE         NODE      DESIRED STATE   CURRENT STATE              ERROR
# ay081uome3   helloworld.1  nginx:alpine  manager1  Running         Preparing 2 seconds ago  
# 16cvore0c96  helloworld.2  nginx:alpine  worker2   Running         Preparing 2 seconds ago
```

##### 服务扩容

```bash
docker service scale helloworld=3
# helloworld scaled to 3
```

> 也可以缩小

##### 回滚服务配置

```bash
docker service rollback $service_id
```

##### 更新服务配置

```bash
docker service update $service_id
```

##### 检查服务配置

```bash
docker service inspect $service_id
```

##### ***查看服务日志***

使用方法

```bash
docker service logs $service_id

  #     --details        Show extra details provided to logs
  # -f, --follow         Follow log output
  #     --no-resolve     Do not map IDs to Names in output
  #     --no-task-ids    Do not include task IDs in output
  #     --no-trunc       Do not truncate output
  #     --raw            Do not neatly format logs
  #     --since string   Show logs since timestamp (e.g. 2013-01-02T13:23:37) or relative (e.g. 42m for 42 minutes)
  #     --tail string    Number of lines to show from the end of the logs (default "all")
  # -t, --timestamps     Show timestamps
```

>等价于

```bash
docker logs $container_id
```

#### 节点查询

```bash
docker node ls
ID                            HOSTNAME                STATUS              AVAILABILITY        MANAGER STATUS      ENGINE VERSION
kb7aqhna2cz9e3i3v4ewkc7sp *   linuxkit-025000000001   Ready               Active              Leader              18.09.2
```

#### 网络

##### 网络查询

```bash
docker network ls
# NETWORK ID         NAME            DRIVER          SCOPE
# 764ff31881e5        bridge          bridge          local
# fbd9a977aa03        host            host            local
# 6p6xlousvsy2        ingress         overlay         swarm
# e81af24d643d        none            null            local
```

docker swarm得网络配置是十分丰富的，而且自带服务发现功能

基于swarm搭配负载均衡工具这些实践还没有做

>To Be Continue
