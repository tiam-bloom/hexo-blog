---
title: docker部署war包
categories:
  - Deploy
tags:
  - docker
  - tomcat
  - war
abbrlink: 93faae0c
date: 2023-04-30 21:41:59
---



# docker部署war包

1. 下载docker

```sh
yum install docker
```

检查是否安装docker

```sh
[root@alibaba-linux home]# docker -v
Emulate Docker CLI using podman. Create /etc/containers/nodocker to quiet msg.
podman version 4.2.0
```

2. 拉取tomcat镜像

```sh
docker pull tomcat  # 默认下载最新版本
docker pull tomcat:8  # 下载tomcat8版本（指定版本）
```

查看镜像

```sh
[root@alibaba-linux home]# docker images
Emulate Docker CLI using podman. Create /etc/containers/nodocker to quiet msg.
REPOSITORY                 TAG         IMAGE ID      CREATED       SIZE
localhost/local-tomcat     latest      f96d2788d42b  2 days ago    495 MB
docker.io/library/tomcat   9           c8f6d60c8268  3 days ago    489 MB
docker.io/library/openjdk  8           b273004037cc  9 months ago  538 MB
```

3. 创建挂载目录

```sh
mkdir -p /home/yonghe/tomcat/webapps/  # -p 递归创建多级目录
```

查看当前所在路径

```sh
[root@alibaba-linux webapps]# pwd
/home/yonghe/tomcat/webapps
```

4. 运行镜像

```sh
docker run -d -p 8080:8080 --name spring-yonghe -v /home/yonghe/tomcat/webapps:/usr/local/tomcat/webapps --restart=always local-tomcat
```

- `-d` , 在后台运行容器并打印容器ID，
- `-p 8080:8080` , 第一个`8080`表示对外映射的端口号, 第二个`8080`表示内部的端口号， Publish a container's port, or a range of ports, to the host (default [])， 翻译： 将容器的端口或一系列端口发布到主机(默认[])
- `-v /home/yonghe/tomcat/webapps:/usr/local/tomcat/webapps`, Mount volumes from the specified container(s), 翻译就是从指定的容器装载卷， `/home/yonghe/tomcat/webapps` 这里代表外部挂载的目录路径， `/usr/local/tomcat/webapps` 这里代表容器内被挂载的目录。
- `--name spring-yonghe` 表示运行的容器名为`spring-yonghe`
- `--restart=always` , 当docker被重启时会自动运行该容器
- `local-tomcat` 表示使用名为`local-tomcat`的镜像运行容器

> tips:
>
> 查看某个命令参数作用：`docker run --help`

查看运行的容器

```sh
[root@alibaba-linux webapps]# docker ps
Emulate Docker CLI using podman. Create /etc/containers/nodocker to quiet msg.
CONTAINER ID  IMAGE                          COMMAND          CREATED         STATUS             PORTS                   NAMES
cdc99de60d2a  localhost/local-tomcat:latest  catalina.sh run  51 minutes ago  Up 51 minutes ago  0.0.0.0:8080->8080/tcp  spring-yonghe
```

5. 上传war包到挂载目录`/home/yonghe/tomcat/webapps`
6. 进入容器内部， 查看目录

```sh
[root@alibaba-linux webapps]# docker exec -it spring-yonghe /bin/bash
Emulate Docker CLI using podman. Create /etc/containers/nodocker to quiet msg.
# 注意命令前提示符变化， 表示已进入
root@cdc99de60d2a:/usr/local/tomcat# ll
total 172
drwxr-xr-x 1 root root  4096 Apr 28 07:51 ./
drwxr-xr-x 1 root root  4096 Apr 26 21:35 ../
drwxr-xr-x 2 root root  4096 Apr 26 21:37 bin/
-rw-r--r-- 1 root root 19992 Apr 13 08:10 BUILDING.txt
drwxr-xr-x 1 root root  4096 Apr 28 07:50 conf/
-rw-r--r-- 1 root root  6210 Apr 13 08:10 CONTRIBUTING.md
drwxr-xr-x 2 root root  4096 Apr 26 21:37 lib/
-rw-r--r-- 1 root root 57092 Apr 13 08:10 LICENSE
drwxrwxrwt 1 root root  4096 Apr 30 11:55 logs/
drwxr-xr-x 2 root root  4096 Apr 26 21:37 native-jni-lib/
-rw-r--r-- 1 root root  2333 Apr 13 08:10 NOTICE
-rw-r--r-- 1 root root  3398 Apr 13 08:10 README.md
-rw-r--r-- 1 root root  6901 Apr 13 08:10 RELEASE-NOTES
-rw-r--r-- 1 root root 16505 Apr 13 08:10 RUNNING.txt
drwxrwxrwt 2 root root  4096 Apr 26 21:37 temp/
drwxr-xr-x 8 root root  4096 Apr 30 11:59 webapps/
drwxrwxrwt 1 root root  4096 Apr 28 07:51 work/
root@cdc99de60d2a:/usr/local/tomcat# cd webapps
root@cdc99de60d2a:/usr/local/tomcat/webapps# ll
total 30648
drwxr-xr-x  8 root root     4096 Apr 30 11:59 ./
drwxr-xr-x  1 root root     4096 Apr 28 07:51 ../
drwxr-xr-x 16 root root     4096 Apr 30 11:58 docs/
drwxr-xr-x  7 root root     4096 Apr 30 11:57 examples/
drwxr-xr-x  6 root root     4096 Apr 30 11:58 host-manager/
drwxr-xr-x  6 root root     4096 Apr 30 11:59 manager/
drwxr-xr-x  6 root root     4096 Apr 30 11:54 ROOT/
drwxr-x---  6 root root     4096 Apr 30 11:59 spring-yonghe/
-rw-r--r--  1 root root 31343379 Apr 30 08:11 spring-yonghe.war
```

> 若`webapps`里没有 这几个目录， 我是从本地tomcat再上传的，不知道有什么好的解决方案
>
> ```
> drwxr-xr-x 16 root root     4096 Apr 30 11:58 docs/
> drwxr-xr-x  7 root root     4096 Apr 30 11:57 examples/
> drwxr-xr-x  6 root root     4096 Apr 30 11:58 host-manager/
> drwxr-xr-x  6 root root     4096 Apr 30 11:59 manager/
> drwxr-xr-x  6 root root     4096 Apr 30 11:54 ROOT/
> ```





---



若始终无法访问,

1. 检查防火墙

` systemctl stop firewalld` 关闭防火墙

```sh
[root@alibaba-linux ~]# systemctl status firewalld
● firewalld.service - firewalld - dynamic firewall daemon
   Loaded: loaded (/usr/lib/systemd/system/firewalld.service; disabled; vendor preset: enabled)
   Active: inactive (dead)  # 防火墙已关闭
     Docs: man:firewalld(1)
```

2. 检查安全组是否放开端口

![image-20230501095152440](http://qiniu.yujing.fit/typora_img/image-20230501095152440.png)



## 参考文章

- [Docker安装tomcat运行war超详细_docker tomcat war_Hi梅的博客-CSDN博客](https://blog.csdn.net/qq_45502336/article/details/118698858)
- [(2条消息) （八）Docker 部署 Tomcat 并部署 war 包访问_docker发布war并访问_bestcxx的博客-CSDN博客](https://blog.csdn.net/bestcxx/article/details/108607230)
- [在docker下的tomcat容器中部署war包的两种方式_docker tomcat10 war_繁星、晚风的博客-CSDN博客](https://blog.csdn.net/qq_39530754/article/details/82909777)