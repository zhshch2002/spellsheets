---
title: Docker CLI
category: Devops
layout: sheet
updated: 2020-07-22
tags: [Featured]
---

Manage images
-------------

### `docker build`

```yml
docker build [options] .
  -t "app/container_name"    # 名称
  --build-arg APP_HOME=$APP_HOME    # Set build-time variables
```

从Dockerfile构建镜像。


### `docker run`

```yml
docker run [options] IMAGE
  # see `docker create` for options
```

#### Example

```
$ docker run -it debian:buster /bin/bash
```
创建Docker容器并执行命令。

Manage containers
-----------------

### `docker create`

```yml
docker create [options] IMAGE
  -a, --attach               # attach stdout/err
  -i, --interactive          # attach stdin (interactive)
  -t, --tty                  # pseudo-tty
      --name NAME            # name your image
  -p, --publish 5000:5000    # 端口映射
      --expose 5432          # 为容器间链接暴露端口
  -P, --publish-all          # 对外暴露全部端口
      --link container:alias # 连接另一个镜像
  -v, --volume `pwd`:/app    # 挂载文件系统
  -e, --env NAME=hello       # 传入环境变量
```


#### Example

```
$ docker create --name app_redis_1 \
  --expose 6379 \
  redis:3.0.2
```

从镜像创建容器但不启动。

### `docker exec`

```yml
docker exec [options] CONTAINER COMMAND
  -d, --detach        # 后台运行
  -i, --interactive   # 链接stdin
  -t, --tty           # 可交互
```

#### Example

```
$ docker exec app_web_1 tail logs/development.log
$ docker exec -t -i app_web_1 rails c
```

在容器中执行命令。


### `docker start`

```yml
docker start [options] CONTAINER
  -a, --attach        # attach stdout/err
  -i, --interactive   # attach stdin

docker stop [options] CONTAINER
```

### `docker ps`

```
$ docker ps        # 列出容器
$ docker ps -a     # 列出全部容器（包括停止的）
$ docker kill $ID  # 销毁容器
```

### `docker logs`

```
$ docker logs $ID
$ docker logs $ID 2>&1 | less
$ docker logs -f $ID   # 持续输出最新日志
```

查看容器的日志输出。

Images
------

### `docker images`

```sh
$ docker images
  REPOSITORY   TAG        ID
  ubuntu       12.10      b750fe78269d
  me/myapp     latest     7b2431a8d968
```

```sh
$ docker images -a   # 也显示中间镜像
```

### `docker rmi`

```yml
docker rmi b750fe78269d
```

删除镜像。

## 清理

### 全部清理

```sh
docker system prune
```

清理镜像，容器，卷和网络（即未被容器使用的资源）。

```sh
docker system prune -a
```

删除所有停止的容器和所有未使用的镜像（不未被容器使用的镜像）。

### Containers

```sh
# Stop all running containers
docker stop $(docker ps -a -q)

# Delete stopped containers
docker container prune
```

### Images

```sh
docker image prune [-a]
```

删除全部镜像。

### Volumes

```sh
docker volume prune
```

删除全部卷。

Also see
--------

 * [Getting Started](http://www.docker.io/gettingstarted/) _(docker.io)_
