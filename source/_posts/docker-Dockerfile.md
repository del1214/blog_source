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

#### WORKDIR

```Dockerfile
WORKDIR /path/to/workdir
```

`WORKDIR`指令设置工作目录。如果目录不存在则会创建。`RUN`, `CMD`, `ENTRYPOINT`, `COPY`, `ADD`都会受到影响



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

#### ENV

`ENV`指指令用来设置环境变量供之后的构建使用

```Dockerfile
ENV <key> <value>
ENV <key>=<value> ...
```

`ENV <key> <value>`设置一个变量,空格前的字符串设置为变量名,空格后的整个字符串设置为值,即便后面字符串中间有空格也被包含在内

`ENV <key>=<value> ...`允许一次设置多个变量，可以在值中间使用空格符

使用`ENV`设置环境变量会对镜像造成持久化的影响

##### 替换ENV设置的方法

`docker run --env key=value`

##### 不造成持久化影响

`RUN <key>=<value> <command>`

#### ARG

`ARG`命令用来定义变量，用户可以在构建镜像时传递参数

##### 默认值

设置一个默认值，构建时没有传递值就使用默认的

```Dockerfile
FROM mysql
ARG user=myuser
ARG buildno=1
```

##### 作用域

`ARG` 定义从当前行生效，并不从`Dockerfile`启动时或命令行调用时就生效

```Dockerfile
FROM busybox
USER ${user:-some_user}
ARG user
USER $user
```

```bash
docker build --build-arg user=what_user .
```

`USER`在第二行为`some_user`
`USER`在第四行为`what_user`

##### ENV与ARG区别

`ENV` 是对容器设置的环境变量，而`ARG`是编译变量.`ENV`值可覆盖`ARG`值，`ENV`值始终保持在镜像中

#### ADD

Docker团队推荐使用`COPY`，😊

#### COPY

```Dockerfile
COPY [--chown=<user>:<group>] <src>... <dest>
COPY [--chown=<user>:<group>] ["<src>",... "<dest>"]
```

`COPY`把`<src>`的文件和目录拷贝到`<dest>`中

支持通配符选择器

```Dockerfile
COPY hom* /mydir/        # 添加"hom"开头的所有文件
COPY hom?.txt /mydir/    # ?匹配任意字符
```

* `<src>`路径必须在上下文(context)之内，不能超出范围
* `<src>`使用相对路径
* `<dest>`可以相对路径(相对WORKDIR)和绝对路径

#### VOLUME

```Dockerfile
VOLUME ["/data"]
```

`VOLUME`指令用来声明容器中的某个目录需要映射到某个数据卷`volume`

> 通过 VOLUME 指令创建的挂载点，无法指定主机上对应的目录，是自动生成的。

要指定宿主机目录需要配合`docker run -v`等命令

#### EXPOSE

```Dockerfile
EXPOSE <port> [<port>/<protocol>...]
```

`EXPOSE`指令通知容器在运行时监听指定的端口，可以明确指定是监听TCP还是UDP默认是TCP

`EXPOSE`指令并不能真正的暴露端口,只是一种指示作用。真正暴露端口还要和`-p`,`-P`,`--link`指令对应起来才可以使用

* `-p`

```bash
docker run -p 8080:80/tcp -p 8080:80/udp ...
```

>使用-p将容器的80端口绑定到宿主机的8080端口上，还可以分别指定方式

* `-P`

```bash
docker run -P nginx:latest
```

>并将容器的80端口映射到主机随机端口

* `--link`

```bash
docker run -P nginx:latest
```

>添加其他容器链接到这个容器,这时被链接的容器可访问这个端口

排列组合

A 既没有在`Dockerfile`里`Expose`，也没有`run -p`
>启动在这个container里的服务既不能被host主机和外网访问，也不能被link的container访问，只能在此容器内部使用
B 只在`Dockerfile`里`Expose`了这个端口
>启动在这个container里的服务不能被docker外部世界（host和其他主机）访问，但是可以通过container link，被其他link的container访问到
C 同时在`Dockerfile`里`Expose`，又`run -p`
>启动的这个cotnainer既可以被docker外部世界访问，也可以被link的container访问
D 只有`run －p`
>docker做了特殊的隐式转换，等价于情况C，既可以被外部世界访问，也可以被link的container访问到（真对这种情况，原因是docker认为，既然你都要把port open到外部世界了，等价于其他的container肯定也能访问，所以docker做了自动的Expose

#### ENTRYPOINT

下面是mysql `Dockerfile`的例子

```Dockerfile
ENTRYPOINT ["docker-entrypoint.sh"]
# sh文件内略过200+行shell脚本
```

`ENTRYPOINT`更适合用在mysql,nginx这种特定软件发行包里使用。即维护者不希望用户自己执行命令而是调用自己的入口文件

所以不再讨论这一块了

#### CMD

`CMD`指令是设置容器启动后默认执行的命令及其参数

`CMD`指令在`Dockerfile`中只能写一条，如果写了多条只有最后一条会生效

```Dockerfile
# (exec form, 推荐使用)
CMD ["executable","param1","param2"]
#  (ENTRYPOINT参数)
CMD ["param1","param2"]
#  (shell form)
CMD command param1 param2
```

> 作为普通用户`CMD`是每个Dockerfile的最后一条命令，必须保证这条命令不会退出，一旦退出容器也将停止
