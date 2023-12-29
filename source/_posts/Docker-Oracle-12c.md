---
title: Docker Oracle-12c
date: 2022-09-24 16:09:12
tags: 
- Database
- Oracle
---
## 安装

### 1. 获取镜像

```bash
# 查找
docker search oracle

# 拉取
docker pull truevoly/oracle-12c

# 查看
docker images
```

<!-- more -->


### 2. 启动Oracle数据库

更新更新！！！
```
docker run -d --name oracle-12c \
  --privileged  --mount source=oracle-data,target=/u01/app/oracle \
  -p 1521:1521 truevoly/oracle-12c  
```
现在只需要执行上面一句就可以了！！！（意思和下面是一样的）

启动前有个比较坑的地方，需要先执行
```bash
sudo mkdir -p  /u01/app/oracle  && sudo chmod -R a+w /u01/app/oracle
sudo mkdir -p  $(pwd)/.oradata  && sudo chmod -R a+w $(pwd)/.oradata
```

不然会报错：
```
Cannot create directory "/u01/app/oracle/cfgtoollogs/dbca".
```

下面

将主机上上的8079端口映射到docker的8080（登录网页端管理）

将主机上上的1521端口映射到docker的1521（数据库连接端口）

挂载主机目录 $(pwd)/.oradata 到oracle服务器/u01/app/oracle目录，这样oracle数据就保存在本地宿主机上

--privileged 使用root权限，不然创建不了文件

```bash
docker run -d --name oracle-12c \
  --privileged -v $(pwd)/.oradata:/u01/app/oracle \
  -p 8079:8080 -p 1521:1521 truevoly/oracle-12c  
```

### 3. 查看日志

```bash
docker logs -f <id>
```

![logs](logs.png)

到100%之后按ctrl+c退出


### 4. 进入oracle容器

```bash
docker exec -it <id> /bin/bash
```

### 5. 连接Oracle数据库

```
hostname: localhost
port: 1521
sid: xe
service name: xe 
username: system 
password: oracle 
```

### 6. sqlplus

```
su oracle
sqlplus / as sysdba
```

