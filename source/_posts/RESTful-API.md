---
title: RESTful API
date: 2023-06-21 21:22:36
tags: RESTful
cover: 2023-06-21-21-23-36.png
---

## 什么是 RESTful API

RESTful API 是一种软件架构风格、设计风格，可以让软件更加清晰，更简洁，更有层次，可维护性更好

REST 是 Representational State Transfer 的缩写 

即表现层状态转移,uri只是描述那里有数据,而数据怎么解释原来由服务端决定,现在REST API中由客户端决定,
即资源的表现层状态由服务端转移到了客户端

## 如何使用

### RESTful API 请求

    请求 = 动词 + 宾语

动词 使用五种 HTTP 方法，对应 CRUD 操作

宾语 URL 应该全部使用名词复数

过滤信息（Filtering） 如果记录数量很多，API应该提供参数，过滤返回结果。 
?limit=10 指定返回记录的数量 ?offset=10 指定返回记录的开始位置。


| method | uri                   | 语义                       |
| ------ | --------------------- | -------------------------- |
| GET    | /users                | 获取所有用户               |
| POST   | /users                | 创建一个用户               |
| GET    | /users/:id            | 获取某个指定用户的信息     |
| PUT    | /users/:id            | 更新某个指定用户的全部信息 |
| PATCH  | /users/:id            | 更新某个指定用户的部分信息 |
| DELETE | /users/:id            | 删除某个用户               |
| GET    | /users/:id/orders     | 获取某个指定用户的所有订单 |
| DELETE | /users/:id/orders/:id | 删除某个指定用户的指定订单 |

### RESTful API 响应

包括 **HTTP 状态码**和**数据**两部分

| code | 语义                 |
| ---- | -------------------- |
| 1xx  | 相关信息(API 不需要) |
| 2xx  | 操作成功             |
| 3xx  | 重定向               |
| 4xx  | 客户端错误           |
| 5xx  | 服务器错误           |


客户端请求的 HTTP 头的 ACCEPT 属性设成 application/json

服务端返回一个 JSON 对象, 回应的 HTTP 头的 Content-Type 属性设为 application/json

错误处理 如果状态码是4xx，就应该向用户返回出错信息。

返回的信息中将 error 作为键名，出错信息作为键值即可。 {error: "Invalid API key"}

认证 RESTful API 应该是无状态，每个请求应该带有一些认证凭证。推荐使用 JWT 认证，并且使用 SSL

