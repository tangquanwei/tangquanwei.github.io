---
title: Docker Chevereto 图床
tags: 
- Docker 
- Chevereto 图床
date: 2023-07-10 11:48:15
cover:
---

# Docker 安装 Chevereto 图床

有试过用mongoDB存图片，在网页上显示要经过一些列转换。速度还是不如直接交给操作系统管理

下面搭建一个图床），方便引用图片。

## 拉取镜像
```bash
docker pull mariadb && 
docker pull nmtan/chevereto:1.4.1
```
## 创建图片存放目录

```bash
cd ~
mkdir -p .chevereto/app/images 
```

授权
```
sudo chomod -R 777 ./app/
```

## 编辑php的配置

```bash
vim ./app/php.ini
```

写入

```
upload_max_filesize = 100M
post_max_size = 100M
memory_limit = 3072M
max_execution_tim = 180
```



## 编辑docker-compose配置

```bash
vim ~/.chevereto/docker-compose.yml

```

写入

```docker-compose
---
version: '3'
 
services:
  db:
    image: mariadb
    volumes:
      - ./db:/var/lib/mysql:rw
    restart: always
    networks:
      - default
    environment:
      MYSQL_ROOT_PASSWORD: 021009 # 按需更改
      MYSQL_DATABASE: chevereto # 按需更改
      MYSQL_USER: chevereto # 按需更改
      MYSQL_PASSWORD: 021009 # 按需更改
 
  app:
    image: nmtan/chevereto:1.4.1 # 固定为1.4.1，个人感觉这个版本最好用
    restart: always
    ports:
      - 7777:80 # 按需更改
    networks:
      - default
    environment:
      CHEVERETO_DB_HOST: db
      CHEVERETO_DB_NAME: chevereto # 与db的设置一一对应
      CHEVERETO_DB_USERNAME: chevereto # 与db的设置一一对应
      CHEVERETO_DB_PASSWORD: 021009 # 与db的设置一一对应
    volumes:
      - ./app/images:/var/www/html/images:rw
      # - ./app/content:/var/www/html/content:rw
      - ./app/php.ini:/usr/local/etc/php/php.ini:ro
      # - ./app/app/routes:/var/www/html/app/routes:rw
    depends_on:
      - db
 
networks:
  default:
    name: chevereto
```

## 启动

```bash
docker-compose up -d
```

## 查看日志
```bash
docker-compose logs -f
```
