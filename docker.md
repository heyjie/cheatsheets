---
title: Docker CLI
category: Devops
layout: 2017/sheet
---


### `docker build`

```yml
docker build [options] .
  -t, --tag "app/container_name"    # 名称和可选格式的标签
  --build-arg APP_HOME=$APP_HOME    # 设置构建时变量
  --file,-f                         # Dockerfile 的名称
  --pull                            # 始终尝试拉取更新版本的镜像
```

#### 例子

```
$ docker build --pull -f "Dockerfile" -t cheatsheets:latest "." 
```

从 Dockerfile 创建镜像

### `docker run`

```yml
docker run [options] IMAGE
  # 见 `docker create` 中的选项
```

#### 例子

```
$ docker run -it debian:buster /bin/bash
```
在镜像中运行一个命令。

容器 Container
-----------------

### `docker create`

```yml
docker create [options] IMAGE
  -a, --attach               # 附加到 STDIN、STDOUT 或 STDERR 上
  -i, --interactive          # 附加标准输入 STDIN（交互式）
  -t, --tty                  # 伪终端
      --name NAME            # 为容器指定一个名称
  -p, --publish 5000:5000    # 端口映射（主机：容器）
      --expose 5432          # 将端口暴露给链接的容器
  -P, --publish-all          # 发布所有端口
      --link container:alias # 添加到另一个容器的链接
  -v, --volume `pwd`:/app    # 挂载（需要绝对路径）
  -e, --env NAME=hello       # 环境变量
      --restart              # 容器退出时要应用的重启策略
```

#### 例子

```
$ docker create --name app_redis_1 \
  --expose 6379 \
  redis:3.0.2
```

从镜像中创建一个容器

### `docker exec`

```yml
docker exec [options] CONTAINER COMMAND
  -d, --detach        # 在后台运行
  -i, --interactive   # 标准输入
  -t, --tty           # 交互式终端
```

#### 例子

```
$ docker exec app_web_1 tail logs/development.log
$ docker exec -t -i app_web_1 rails c
```

在容器中运行一条命令


### `docker start`

```yml
docker start [options] CONTAINER
  -a, --attach        # 附加 STDOUT/STDERR 和转发信号
  -i, --interactive   # 附加容器的标准输入 STDIN

docker stop [options] CONTAINER
  -s, --signal        # 发送给容器的信号
  -t, --time          # 停止容器前要等待的秒数

docker restart [OPTIONS] CONTAINER
  # 见 `docker stop` 中的选项

docker pause CONTAINER     # 暂停一个或多个容器中的所有进程
docker unpause CONTAINER   # 解除一个或多个容器内所有进程的暂停状态
```

启动或停止一个容器


### `docker ps`

```yml
docker ps [OPTIONS]
  -a, --all            # 显示所有容器（默认只显示正在运行的容器）
  -q, --quiet          # 只显示容器的ID
```

```sh
$ docker ps -a
$ docker kill $ID    # 关闭一个或多个正在运行的容器
```

使用 ps/kill 管理容器


### `docker logs`

```sh
$ docker logs $ID
$ docker logs $ID 2>&1 | less
$ docker logs -f $ID    # 跟踪日志输出
```

看看在一个容器中记录了什么


镜像 Images
------

### `docker images`

```sh
$ docker images
  REPOSITORY   TAG        ID
  ubuntu       12.10      b750fe78269d
  me/myapp     latest     7b2431a8d968
```

```sh
$ docker images -a   # 列出所有镜像（包含中间镜像层）
$ docker images -q   # 只显示镜像id
```

```yml
docker search [OPTIONS] CONTAINER    # 搜索Docker Hub中的镜像
  --no-trunc         # 显示完整的镜像信息
```

```yml
docker pull [OPTIONS] NAME[:TAG|@DIGEST]    # 下载镜像
```

管理镜像

### `docker rmi`

```yml
docker image rm [OPTIONS] IMAGE
  -f                  # 强制删除    
docker rmi [OPTIONS] IMAGE       # 删除镜像(docker image rm的简化写法)
  # 见 `docker image rm` 中的选项
docker rmi `docker images -q`    # 删除所有的镜像
```

删除镜像

## 清理

### 清理所有

```sh
docker system prune
```

清理挂起的镜像、容器、数据卷和网络（即与容器无关的）

```sh
docker system prune -a
```

另外删除任何停止的容器和所有未使用的镜像（不仅仅是挂起的镜像）

### 容器

```sh
# 停止所有正在运行的容器
docker stop $(docker ps -a -q)

# 删除已停止的容器
docker container prune
```

### 镜像

```sh
docker image prune [-a]
```

删除所有的镜像

### 数据卷

```sh
docker volume prune
```

删除所有数据卷

另请参见
--------

 * [开始使用 Docker](https://www.docker.com/get-started/) _(docker.io)_
 * [Docker Hub 官网](https://hub.docker.com/) _(hub.docker.com)_
