---
title: React + Websocket 实现网络聊天室
date: 2023-02-09 15:23:37
tags: 
- JavaScript
- React
- WebScoket
cover:
---

## 需求

1. 注册
2. 登陆
3. 群聊
4. 私聊
5. 发送文件

## 设计

### 技术栈

使用 Oracle 数据库存储数据

使用 SpringBoot 搭建 Websocket 服务器

使用 Websocket 协议传输数据

使用 React.js 编写前端页面


### 数据对象

用户登陆信息 LoginInfo
{
  id :number(10),
  password :varchar(20),

}

用户信息LoginInfo
{
  name  :varchar(20),
  avatar:
  email :
  phone:
  ...
}

消息 Message
{
  from:
  to:
  data:
  dataType:
  date:
}

DataType
{
  TEXT,
  IMAGE,
  VIDEO,
}




## 前端实现

### 将 UI 拆解为组件层级结构 

![](2023-06-09-16-16-42.png)

### 使用 React 构建一个静态版本
![](2023-06-11-20-41-29.png)

## 后端实现


## 测试

使用 Postman 进行接口测试

## 部署

使用 Docker 部署服务到腾讯云

聊天静态文件用 Nginx 

