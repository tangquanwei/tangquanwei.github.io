---
title: OpenGauss Database
date: 2023-04-07 14:41:03
tags: 
- OpenGauss
- Database
---
# OpenGauss Database

## 安装
```bash

docker run --name opengauss --privileged=true -d -e GS_PASSWORD=Tq@021009 -p 5432:5432 enmotech/opengauss:latest

docker run --name opengauss --privileged=true -d -e GS_PASSWORD=Tq@021009 -p 5432:5432 opengauss/opengauss

gsql -d postgres -U gaussdb -W 'Tq@021009' -h host_ip -p 5432

su - omm

gsql

```

```
步骤 1 以操作系统用户omm登录数据库主节点。
gs_om -t status --detail

步骤 2 使用如下命令查询openGauss状态：

若要查询某主机上的实例状态，请在命令中增加“-h”项。示例如下：。示例如下：
gs_om -t status -h plat2 --detail

其中，plat2为待查询主机的名称。

```

步骤 1 以操作系统用户omm登录数据库主节点。

步骤 2 使用以下命令启动openGauss。
gs_om -t start

启动openGauss语法
gs_om -t start [-h HOSTNAME] [-D dataDir] [--time-out=SECS] [--security-mode=MODE] [-l LOGFILE]

说明：双机启动必须以双机模式启动, 若中间过程以单机模式启动, 则必须修复才能恢复双机关系, 用gs_ctl build进行修复。

步骤 1 以操作系统用户omm登录数据库主节点。

步骤 2 使用以下命令停止openGauss。
gs_om -t stop

停止openGauss语法：
gs_om -t stop [-h HOSTNAME] [-D dataDir]  [--time-out=SECS] [-m MODE] [-l LOGFILE]


系统日志存储位置：数据库节点的运行日志放在“/var/log/gaussdb/用户名/pg_log”中各自对应的目录下。
数据库节点运行日志的命名规则：postgresql-创建时间.log
