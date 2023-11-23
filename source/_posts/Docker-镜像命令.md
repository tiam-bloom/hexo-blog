---
title: Docker 镜像命令
cover: ''
tags:
  - Docker
categories: 
 - Docker
abbrlink: adc983fe
date: 2023-01-28 17:38:35
---

# 基本概念

- **镜像（Image）**：Docker 镜像（Image），就相当于是一个 root 文件系统。比如官方镜像 ubuntu:16.04 就包含了完整的一套 Ubuntu16.04 最小系统的 root 文件系统。
- **容器（Container）**：镜像（Image）和容器（Container）的关系，就像是面向对象程序设计中的类和实例一样，镜像是静态的定义，容器是镜像运行时的实体。容器可以被创建、启动、停止、删除、暂停等。
- **仓库（Repository）**：仓库可看成一个代码控制中心，用来保存镜像。

# 帮助

## 查看帮助

- `$ docker`

```shell
[root@1674615372401 ~]# docker

Usage:  docker COMMAND

A self-sufficient runtime for containers

Options:
      --config string      Location of client config files (default "/root/.docker")
  -D, --debug              Enable debug mode
      --help               Print usage
  -H, --host list          Daemon socket(s) to connect to (default [])
  -l, --log-level string   Set the logging level ("debug", "info", "warn", "error", "fatal") (default "info")
      --tls                Use TLS; implied by --tlsverify
      --tlscacert string   Trust certs signed only by this CA (default "/root/.docker/ca.pem")
      --tlscert string     Path to TLS certificate file (default "/root/.docker/cert.pem")
      --tlskey string      Path to TLS key file (default "/root/.docker/key.pem")
      --tlsverify          Use TLS and verify the remote
  -v, --version            Print version information and quit

Management Commands:
  container   Manage containers
  image       Manage images
  network     Manage networks
  node        Manage Swarm nodes
  plugin      Manage plugins
  secret      Manage Docker secrets
  service     Manage services
  stack       Manage Docker stacks
  swarm       Manage Swarm
  system      Manage Docker
  volume      Manage volumes

Commands:
  attach      Attach to a running container
  build       Build an image from a Dockerfile  # 从Dockerfile构建镜像
  commit      Create a new image from a container's changes
  cp          Copy files/folders between a container and the local filesystem
  create      Create a new container  # 创建一个新的容器
  diff        Inspect changes on a container's filesystem
  events      Get real time events from the server
  exec        Run a command in a running container
  export      Export a container's filesystem as a tar archive
  history     Show the history of an image
  images      List images  # 列出所有镜像
  import      Import the contents from a tarball to create a filesystem image
  info        Display system-wide information
  inspect     Return low-level information on Docker objects
  kill        Kill one or more running containers  # 杀死一个或多个正在运行的容器
  load        Load an image from a tar archive or STDIN
  login       Log in to a Docker registry
  logout      Log out from a Docker registry
  logs        Fetch the logs of a container  # 获取容器的日志
  pause       Pause all processes within one or more containers  # 暂停一个或多个容器中的所有进程
  port        List port mappings or a specific mapping for the container
  ps          List containers  # 列出所有容器
  pull        Pull an image or a repository from a registry
  push        Push an image or a repository to a registry  
  rename      Rename a container  # 重命名容器
  restart     Restart one or more containers  # 重启一个或多个容器
  rm          Remove one or more containers # 删除一个或多个容器
  rmi         Remove one or more images # 删除一个或多个镜像
  run         Run a command in a new container  # 在新容器中运行命令
  save        Save one or more images to a tar archive (streamed to STDOUT by default)
  search      Search the Docker Hub for images
  start       Start one or more stopped containers  # 启动一个或多个停止的容器
  stats       Display a live stream of container(s) resource usage statistics
  stop        Stop one or more running containers  # 停止一个或多个正在运行的容器
  tag         Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE
  top         Display the running processes of a container  # 显示容器正在运行的进程
  unpause     Unpause all processes within one or more containers
  update      Update configuration of one or more containers
  version     Show the Docker version information
  wait        Block until one or more containers stop, then print their exit codes

Run 'docker COMMAND --help' for more information on a command.
```

## 查看命令用法

- `$ docker [命令] --help`

```shell
[root@1674615372401 ~]# docker images --help

Usage:  docker images [OPTIONS] [REPOSITORY[:TAG]]

List images

Options:
  -a, --all             Show all images (default hides intermediate images)
      --digests         Show digests
  -f, --filter filter   Filter output based on conditions provided
      --format string   Pretty-print images using a Go template
      --help            Print usage
      --no-trunc        Don't truncate output
  -q, --quiet           Only show numeric IDs
```

# 镜像命令



## 搜索镜像

- `docker search [keywords]`

```shell
[root@1674615372401 ~]# docker search hello
INDEX       NAME                                           DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
docker.io   docker.io/hello-world                          Hello World! (an example of minimal Docker...   1962      [OK]       
docker.io   docker.io/hello-seattle                        Hello from DockerCon 2016 (Seattle)!            13        [OK]       
docker.io   docker.io/rancher/hello-world                                                                  4                    
docker.io   docker.io/ibmcom/helloworld                    A sample used by IBM Cloud Code Engine          2       ...
```

## 下载镜像

- `docker pull [image name]`

```shell
[root@1674615372401 ~]# docker pull hello-world
Using default tag: latest
Trying to pull repository docker.io/library/hello-world ... 
latest: Pulling from docker.io/library/hello-world
2db29710123e: Pull complete 
Digest: sha256:2498fce14358aa50ead0cc6c19990fc6ff866ce72aeb5546e1d59caac3d0d60f
Status: Downloaded newer image for docker.io/hello-world:latest
```

## 查询所有镜像

```shell
[root@1674615372401 ~]# docker images
REPOSITORY              TAG                 IMAGE ID            CREATED             SIZE
test                    1.0                 ae784e527383        26 minutes ago      598 MB
<none>                  <none>              092cd1cb0519        55 minutes ago      598 MB
docker.io/openjdk       8                   e24ac15e052e        13 months ago       526 MB
docker.io/mysql         latest              3218b38490ce        13 months ago       516 MB
docker.io/hello-world   latest              feb5d9fea6a5        16 months ago       13.3 kB
docker.io/java          8                   d23bdf5b1b1b        6 years ago         643 MB
```

## 删除镜像

- `docker rmi [image id]`

```shell
[root@1674615372401 ~]# docker rmi 092cd1cb0519
Deleted: sha256:092cd1cb05190dcf6dea044dd92dc3b8a5f5bc8077157c88e31a49f3891d756f
Deleted: sha256:fcf3f071fde7fba8fa6ffbb8fb6e3e911b1e7673bcaa9fa0151f604cf56dde39
Deleted: sha256:841aa62063a5a204f52201bf009f8d19ec9748b374f6d223c07e2c75082271c4
Deleted: sha256:db3ab2e6593987d605b87e2402cd0db8be0cbbaa1f0c098a4c65c2d6e15169fb
Deleted: sha256:cb19cc4ba12ebe123c8c42be0da1a6fe75fa011bbe66b81682561776f2d394eb
```

- 报错:  `image is being used by stopped container`, 执行以下命令再删除

```bash
docker ps -a | grep "Exited" | awk '{print $1 }'|xargs docker stop
docker ps -a | grep "Exited" | awk '{print $1 }'|xargs docker rm
docker images|grep none|awk '{print $3 }'|xargs docker rmi
```



## 运行容器

- **`-t`:** 在新容器内指定一个伪终端或终端。
- **`-i`:** 允许你对容器内的标准输入 (STDIN) 进行交互。
- `-d`:   指定容器的运行模式, 默认以进程方式运行

```shell
[root@1674615372401 ~]# docker run -itd --name mysql-test -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 mysql
2b237156961bba56778b13e9b3f3e9486497d4d52b70391d11daed0a7bbc5f36
```

```shell
[root@1674615372401 ~]# docker run hello-world

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/

```



## 查看运行

- `-a` 查看历史运行记录

```shell
[root@1674615372401 ~]# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                               NAMES
2b237156961b        mysql               "docker-entrypoint..."   2 minutes ago       Up 2 minutes        0.0.0.0:3306->3306/tcp, 33060/tcp   mysql-test
a41e7e889a1a        test:1.0            "java -Djava.secur..."   35 minutes ago      Up 35 minutes       0.0.0.0:8080->8081/tcp              springboot
```

**STATUS:** 容器状态。

状态有7种：

- created（已创建）
- restarting（重启中）
- running 或 Up（运行中）
- removing（迁移中）
- paused（暂停）
- exited（停止）
- dead（死亡）

https://www.runoob.com/docker/docker-tutorial.html

## 停止运行容器

- `docker stop [CONTAINER ID ]`

## 进入容器与退出

- `docker exec [CONTAINER ID ]  /bin/bash`  指定使用的控制台地址/bin/bash

```shell
[root@1674615372401 ~]# docker exec -it 2b237156961b /bin/bash
# 发生变化
root@2b237156961b:/# exit
exit
# 恢复回来
[root@1674615372401 ~]# 
```

## 查看容器运行日志

- `docker logs -f [CONTAINER ID ]`
- `-f` 实时显示日志

```shell
[root@1674615372401 ~]# docker logs -f a41e7e889a1a
  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::                (v2.5.6)
 ....
```

## 重启容器

启动一个或多个已经停止的容器

- `docker start [name]`

## 构建镜像

- `Dockerfile`命令说明:  https://www.runoob.com/docker/docker-dockerfile.html
- `$ docker build -t nginx:v3 .` 
- **-t** ：指定要创建的目标镜像名
- `v3` 表示版本标签
- 最后一个点表示`Dockerfile`所在的路径, 在当前路径同目录下
