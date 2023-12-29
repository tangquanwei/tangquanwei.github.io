---
title: docker 最佳实践
date: 2022-09-24 15:43:36
tags: 
- Linux
- Docker
--- 
# Docker

## Docker镜像

<!-- more -->
* 搜索

  cocker search ubuntu

* 拉取

  docker pull ubuntu

* 运行

  docker run -it --name ubuntu-test ubuntu /bin/bash

* 后台运行

  docker run -itd  ubuntu /bin/bash

参数：

  -i: 交互式操作。
  -t: 终端。
  -d: 参数默认不会进入容器，想要进入容器需要使用指令 docker exec
  ubuntu: ubuntu 镜像。
  /bin/bash：放在镜像名后的是命令，这里是交互式 Shell /bin/bash。

在使用 -d 参数时，容器启动后会进入后台。此时想要进入容器，可以通过以下指令进入：

  docker attach <容器 ID>

or

  docker exec -it <容器 ID> /bin/bash # 此命令会退出容器终端，但不会导致容器的停止

## Docker容器

* 退出容器

  Ctrl + P + Q # 不停止运行

  exit # 会停止运行

* 查看所有的容器

  docker ps -a

* 删除容器

  docker rm -f <容器 ID>

* 停止容器

  docker stop <容器 ID>

* 启动一个已停止的容器

  docker start <容器 ID>

## Docker网络

Docker的网络主要用于连接几个有关联的容器同时隔离不相关的。

Docker的网络可以分为下面4种:

  bridge 桥接（默认）
    通过 IP 或 用户自定义网络访问

  host 容器与宿主机共享网络

  none 禁用网络

  用户自定义网络

Docker 网络相关的命令

```
docker network ls 
列出运行在本地Docker主机上的全部网络

docker network create <网络名>
创建新的Docker自定义网络,可以使用-d 参数指定驱动（网络类型）

创建了网络就可以在创建容器时使用：--network <网络名> 将容器添加到网络

docker network create -d overlay overnet 
创建一个新的名为overnet的覆盖网络，其采用的驱动为Docker Overlay 

docker network inspect <网络名或id>
提供Docker网络的详细配置信息

docker network prune 
删除Docker主机上全部未使用的网络

docker network rm <网络名或id>
删除Docker主机上指定网络
```

  
## Docker卷

Docker数据主要分为两类，持久化的与非持久化的。

持久化数据是需要保存到属主机磁盘上的数据。例如数据库数据。非持久化数据是不需要保存的那些数据。

每个Docker容器都有自己的非持久化存储。非持久化存储自动创建，从属于容器，生命周期与容器相同。这意味着删除容器也会删除全部非持久化数据。

如果希望自己的容器数据保留下来（持久化），则需要将数据存储在卷上。

每个容器都被自动分配了本地存储：
在Linux系统中，该存储的目录在/var/lib/docker/<storage-driver>/ 之下，是容器的一部分。
在Windows系统中位于C\ProgramData\Docker\windowsfilter\ 目录之下。

Docker 卷相关的命令

```
docker volume create <卷名>
命令用于创建新卷。默认情况下，新卷创建使用local 驱动，但是可以通过-d 参数来指定不同的驱动

docker volume ls 
会列出本地Docker主机上的全部卷

docker volume inspect <卷名或id>
用于查看卷的详细信息

docker volume prune 
会删除未被容器或者服务副本使用的全部卷

docker volume rm <卷名或id>
删除未被使用的指定卷
```

## 普通用户使用 Docker 

普通用户使用docker命令需要加上sudo,原因是：

Docker的守护线程绑定的是unix socket，而不是TCP端口，这个套接字默认属于root，其他用户可以通过sudo去访问这个套接字文件。
所以docker服务进程都是以root账户运行。

解决的方式是：

创建docker用户组，把应用用户加入到docker用户组里面。只要docker组里的用户都可以直接执行docker命令。
可以先通过指令查看是否有用户组：
```bash
cat /etc/group | grep docker
```

如果有就跳过第一步！

第一步：创建docker用户组
```bash
sudo groupadd docker 
```

第二步：将用户加入到用户组
```bash
sudo usermod -aG docker $USER
```

第三步：检查是否有效
```bash
cat /etc/group
```

第四步：重启docker-daemon
```bash
sudo systemctl restart docker
```

第五步：给docker.sock添加权限
```bash
sudo chmod a+rw /var/run/docker.sock
```