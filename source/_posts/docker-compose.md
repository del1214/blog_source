---
title: docker-compose
date: 2019-06-18 16:29:18
categories:
  - Docker
tags:
  - Docker
---

## docker-compose

我曾经跨过山河大海、也穿过人山人海！！看过了前面那么多知识概念，自己的docker镜像终于跑起来了。但总感觉少了点什么，那就是老敲命令太tm烦了。尤其是container每次还的先删掉才能重新启动。`docker run`那么多参数哥真的记不住啊。一个项目要依赖很多服务，又要部署超多实例，没有带给我们丝毫的便利。于是有人来拯救我们了！

使用docker-compose可以轻松、高效的管理容器，它是一个定义和运行多容器Docker应用的工具。使用YAML文件来配置应用服务，只需要一行命令就可以创建运行配置的服务。

### Compose文件

docker-compose主要围绕compose文件定义工作,从发布到目前一共有下面这些版本
<table>
  <thead>
    <tr>
      <th><strong>Compose文件版本</strong></th>
      <th><strong>Docker Engine版本需求</strong></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>3.7</td>
      <td>18.06.0+</td>
    </tr>
    <tr>
      <td>3.6</td>
      <td>18.02.0+</td>
    </tr>
    <tr>
      <td>3.5</td>
      <td>17.12.0+</td>
    </tr>
    <tr>
      <td>3.4</td>
      <td>17.09.0+</td>
    </tr>
    <tr>
      <td>3.3</td>
      <td>17.06.0+</td>
    </tr>
    <tr>
      <td>3.2</td>
      <td>17.04.0+</td>
    </tr>
    <tr>
      <td>3.1</td>
      <td>1.13.1+</td>
    </tr>
    <tr>
      <td>3.0</td>
      <td>1.13.0+</td>
    </tr>
    <tr>
      <td>2.4</td>
      <td>17.12.0+</td>
    </tr>
    <tr>
      <td>2.3</td>
      <td>17.06.0+</td>
    </tr>
    <tr>
      <td>2.2</td>
      <td>1.13.0+</td>
    </tr>
    <tr>
      <td>2.1</td>
      <td>1.12.0+</td>
    </tr>
    <tr>
      <td>2.0</td>
      <td>1.10.0+</td>
    </tr>
    <tr>
      <td>1.0</td>
      <td>1.9.1.+</td>
    </tr>
  </tbody>
</table>

不同版本的Compose文件格式要求不同的Docker Engine和docker-compose版本。之前就发生过Compose文件版本过高，docker-compose版本过低无法运行的窘境

#### Compose文件版本选择

> 可以使用最新的。也可以按阿里云一类的要求用对应版本的格式

### Compose文件的编写

Compose文件使用`YAML`格式编写，这个格式在python,ruby等语言中比较流行

让我们来看看大搜车easy-mock服务的`Compose`文件

```yml
# 声明版本
version: '3'

# service配置 - 固定格式
services:
  # 服务名称-叫什么都行，自己起名字
  mongodb:
    # 依赖镜像
    image: mongo:3.4
    # 数据卷配置
    # 将容器/data/db目录映射到宿主机上下文的./data/db目录，这个目录在宿主机可能会有权限问题，一般需提前创建好并chown 555 -R ./data/db解决.也可以配置service的privileged: true解决,但使用了这个文件夹权限会是root级别
    volumes:
      - './data/db:/data/db'
    # 配置网络
    networks:
      # 容器可以发现easy-mock网络
      easy-mock:
        # 配置容器网络别名
        aliases:
          # webapp可以通过hostname mongodb来找到这台服务器,数据库链接字符串变成了mongodb://mongodb/easy-mock
          - mongodb
    # swarm 模式失效
    # 一直重启
    restart: always
    # swarm 模式生效
    deploy:
      # 实例个数 1，webserver可以配多个
      replicas: 1
      # 资源配置
      resources:
        # 固定格式
        limits:
          # cpu单核的20%处理能力
          cpus: ".2"
          # 不能超过50兆内存
          memory: 50M
  redis:
    image: redis:4.0.6
    # 替换掉Dockerfile中的CMD指令
    command: redis-server --appendonly yes
    volumes:
      - './data/redis:/data'
    networks:
      easy-mock:
        # 其他同一网络下的可用 host redis来访问
        aliases:
          - redis
    restart: always
    deploy:
      replicas: 1
      resources:
        limits:
          cpus: "0.3"
          memory: 50M

  web:
    image: easymock/easymock:1.6.0
    command: /bin/bash -c "npm start"
    # 将容器的7300端口映射到宿主机7300端口
    ports:
      - 7300:7300
    volumes:
      # 日志地址
      - './logs:/home/easy-mock/easy-mock/logs'
    networks:
      - easy-mock
    restart: always
    deploy:
      replicas: 1
      resources:
        limits:
          cpus: "0.5"
          memory: 200M
# network配置
networks:
  # 创建名叫easy-mock的network
  easy-mock:
```

### 编译自己的Dockerfile

```yml
version: '3'

services:

  webapp:
    # 赋予volume权限
    privileged: true
    # 编译本地的Dockerfile，文件名默认是Dockerfile
    build: .
    ports:
      - "8888:3000"
    volumes:
      # 下面这个有个大坑就是会让自己Dockerfile的COPY到WORKDIR的内容全部消失，😂
      # - ./:/code  加入这个会让dockerfile npm i 产生的 node_modules消失
      - ./logs:/code/logs

```

### [详细的配置](https://docs.docker.com/compose/compose-file/)

### 使用环境变量

假设有文件 dev.sh

```bash
export NODE_RUN=dev
export NGINX_PORT=8089
export COMPOSE_PROJECT_NAME=oil_mall_admhtml_dev
```

在配置文件中${variable_name}引用

```yml
version: '3'

services:
  webapp:
    build: .
    volumes:
      - ./dist:/code/dist
    environment:
      - NODE_RUN
    ports:
      - ${NGINX_PORT}:80
```

使用方法

```bash
source dev.sh
docker-compose config
```

### 常用命令

#### ***启动***

创建启动container

```bash
docker-compose -f docker-compose.yml up -d --build
# 重要参数
    # -d, --detach               后台运行
    # --build                    开启容器前始终编译镜像
```

#### ***停止***

停止并删除container,networks,images,volumes

```bash
docker-compose -f "docker-compose.yml" down

# 重要参数
  # -v, --volumes               删除volumes
    # --rmi type                删除镜像,all|local 全部或本地未加标签的镜像
```

#### 检查单个容器环境

```bash
docker-compose run web env
```

***单独的docker-compose只用于开发测试
配合 docker swarm 和 docker stack就是用于部署***

### 疑难配置详解

#### command 和 entrypoint

***command 和 entrypoint 最后必须能产生一个不退出进程的PID***

> command 和 entrypoint 指令会替换镜像 Dockerfile 中最后的 CMD 或 ENTRYPOINT 如果不太熟悉官方镜像尽量只传参数不要自己写命令

##### 多行书写

```yml
  command:
    - /bin/sh
    - -c
    - |
        bundle config mirror.https://rubygems.org https://gems.ruby-china.org
        bundle exec rake redmine:plugins:migrate RAILS_ENV=production
        bundle exec rake tmp:cache:clear tmp:sessions:clear RAILS_ENV=production
        /docker-entrypoint.sh passenger start
# 或
  command: ["sh", "-c", "cp -r /usr/src/redmine/public/. /www/public/ && /docker-entrypoint.sh"]
```

### 实战

#### 设置Mysql时区,默认字符集等参数

```yml
services:
  db:
    image: mysql
    restart: always
    command: [
      '--character-set-server=utf8mb4',
      '--collation-server=utf8mb4_unicode_ci',
      '--default-time-zone=+8:00',
      '--default-authentication-plugin=mysql_native_password'
    ]
# 或
services:
  db:
    image: mysql
    restart: always
    command: --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci --default-time-zone=+8:00 --default-authentication-plugin=mysql_native_password
```


>To Be Continue
