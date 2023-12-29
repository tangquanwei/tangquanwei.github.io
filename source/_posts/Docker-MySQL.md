---
title: Docker MySQL
tags: Docker MySQL
date: 2023-07-04 15:46:43
cover:
---

```
docker run \
  --name mysql-8 \
  -d \
  -p 3306:3306 \
  --restart unless-stopped \
  -v ~/.mysql/log:/var/log/mysql \
  -v ~/.mysql/data:/var/lib/mysql \
  -e MYSQL_ROOT_PASSWORD=021009 \
  mysql:8.0.31
```

```
int：整型
double：浮点型，例如double(5,2)表示最多5位，其中必须有2位小数，即最大值为999.99；
char：固定长度字符串类型；    char(10)     'aaa       '  占10位
varchar：可变长度字符串类型； varchar(10)  'aaa'  占3为
text：字符串类型；
blob：字节类型；
date：日期类型，格式为：yyyy-MM-dd；
time：时间类型，格式为：hh:mm:ss
timestamp：时间戳类型 yyyy-MM-dd hh:mm:ss  会自动赋值
datetime:日期时间类型 yyyy-MM-dd hh:mm:ss
```
