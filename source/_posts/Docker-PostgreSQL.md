---
title: PostgreSQL
tags: 
- Docker
- PostgreSQL
date: 2023-08-02 19:27:25
cover: 2023-08-02-19-46-38.png
---

PostgreSQL: The World's Most Advanced Open Source Relational Database

## Docker 安装 [PostgreSQL](https://www.postgresql.org/)

```bash
docker run -d --name postgres \
  -p 5432:5432 \
  -e POSTGRES_PASSWORD=021009 \
  -v /home/quanwei/docker/pgdata:/var/lib/postgresql/data \
  postgres
```
![](2023-08-02-19-37-22.png)

![](2023-08-02-19-36-43.png)

![](2023-08-02-20-01-25.png)

## Docker 安装 pgAdmin

pgAdmin is the most popular and feature rich Open Source administration and development platform for PostgreSQL, the most advanced Open Source database in the world. 

```bash
docker run -d --name pgadmin \
  -e PGADMIN_DEFAULT_EMAIL=postgres@tang.qw \
  -e PGADMIN_DEFAULT_PASSWORD=021009 \
  -p 5050:80 \
  --add-host=host.docker.internal:host-gateway \
  dpage/pgadmin4
```
![](2023-08-02-19-44-15.png)
![](2023-08-02-19-44-54.png)

## 数据类型


| 名字                                    | 别名               | 描述                     |
| --------------------------------------- | ------------------ | ------------------------ |
| bigint                                  | int8               | 有符号八字节整数         |
| bigserial                               | serial8            | 自动递增八字节整数       |
| bit [ (n) ]                             |                    | 固定长度的位字符串       |
| bit varying [ (n) ]                     | varbit [ (n) ]     | 可变长度位字符串         |
| boolean                                 | bool               | 逻辑布尔值（真/假）      |
| box                                     |                    | 平面上的矩形框           |
| bytea                                   |                    | 二进制数据（“字节数组”)  |
| character [ (n) ]                       | char [ (n) ]       | 固定长度的字符串         |
| character varying [ (n) ]               | varchar [ (n) ]    | 可变长度字符串           |
| cidr                                    |                    | IPv4 或 IPv6 网络地址    |
| circle                                  |                    | 平面上的圆               |
| date                                    |                    | 日历日期（年、月、日）   |
| double precision                        | float8             | 双精度浮点数（8 字节）   |
| inet                                    |                    | IPv4 或 IPv6 主机地址    |
| integer                                 | int,int4           | 有符号四字节整数         |
| interval [ fields ] [ (p) ]             |                    | 时间跨度                 |
| json                                    |                    | 文本 JSON 数据           |
| jsonb                                   |                    | 二进制 JSON 数据，已分解 |
| line                                    |                    | 平面上的无限线           |
| lseg                                    |                    | 平面上的线段             |
| macaddr                                 |                    | MAC（媒体访问控制）地址  |
| macaddr8                                |                    | MAC地址（EUI-64 格式）   |
| money                                   |                    | 货币金额                 |
| numeric [ (p, s) ]                      | decimal [ (p, s) ] | 可选精度的精确数字       |
| path                                    |                    | 平面上的几何路径         |
| pg_lsn                                  |                    | PostgreSQL日志序列号     |
| pg_snapshot                             |                    | 用户级交易 ID 快照       |
| point                                   |                    | 平面上的几何点           |
| polygon                                 |                    | 平面上的闭合几何路径     |
| real                                    | float4             | 单精度浮点数（4 字节）   |
| smallint                                | int2               | 有符号双字节整数         |
| smallserial                             | serial2            | 自动递增双字节整数       |
| serial                                  | serial4            | 自动递增四字节整数       |
| text                                    |                    | 可变长度字符串           |
| time [ (p) ] [ without time zone ]      |                    | 一天中的时间（无时区）   |
| time [ (p) ] with time zone	timetz      |                    | 一天中的时间，包括时区   |
| timestamp [ (p) ] [ without time zone ] |                    | 日期和时间（无时区）     |
| timestamp [ (p) ] with time zone        | timestamptz        | 日期和时间，包括时区     |
| tsquery                                 |                    | 文本搜索查询             |
| tsvector                                |                    | 文本搜索文档             |
| uuid                                    |                    | 通用唯一标识符           |
| xml                                     |                    | XML 数据                 |
