---
title: KeyCloak - Open Source Identity and Access Management
tags: 
- KeyCloak 
- Identity
date: 2023-08-02 17:55:57
cover: 2023-08-02-17-57-00.png
---

## Docker 安装

```
docker run --name keycloak -p 8001:8080 -e KEYCLOAK_ADMIN=admin -e KEYCLOAK_ADMIN_PASSWORD=021009 quay.io/keycloak/keycloak:22.0.1 start-dev
```

## 登录管理控制台

转到 [Keycloak 管理控制台](localhost:8000)。

使用之前创建的用户名和密码登录。

## 创建领域(realm)

Keycloak 中的领域等效于租户。每个领域都允许管理员创建独立的应用程序和用户组。
最初，Keycloak 包括一个 master 领域。此领域仅用于管理 Keycloak，而不用于管理任何应用程序。

使用以下步骤创建第一个领域。

打开 Keycloak 管理控制台。

点按左上角的单词 master，然后点按“创建领域”。

在领域名称字段中输入: myrealm

单击创建。
![](2023-08-02-18-02-37.png)

## 创建用户

最初，领域没有用户。使用以下步骤创建用户：

打开 Keycloak 管理控制台。

单击左侧菜单中的用户。

单击创建新用户。

使用以下值填写表单：

用户名：myuser

名字：任何名字

姓氏：任何姓氏

单击创建。

![](2023-08-02-18-03-04.png)

此用户需要密码才能登录。要设置初始密码：

单击页面顶部的凭据。

使用密码填写“设置密码”表单。

将“临时”切换为“关闭”，以便用户在首次登录时无需更新此密码。

![](2023-08-02-18-03-23.png)

## 登录账户控制台

您现在可以登录到帐户控制台以验证此用户是否已正确配置。

打开Keycloak帐户控制台。

使用您之前创建的密码登录。myuser

作为账户控制台中的用户，您可以管理您的账户，包括修改您的个人资料、添加双因素身份验证以及包括身份提供商账户。

![](2023-08-02-18-04-00.png)

## 保护第一个应用程序

要保护第一个应用程序，首先将应用程序注册到 Keycloak 实例：

打开 Keycloak 管理控制台。

单击“客户端”。

单击“创建客户端”

使用以下值填写表单：

客户端类型：OpenID Connect

客户端 ID：myclient

单击下一步

确认已启用标准流。

单击保存。
![](2023-08-02-18-04-29.png)

创建客户端后，对客户端进行以下更新：

向下滚动到“访问设置”。

将有效重定向 URI 设置为https://www.keycloak.org/app/*

将 Web 源设置为https://www.keycloak.org

单击保存。

![](2023-08-02-18-04-49.png)

要确认客户端已成功创建，您可以使用Keycloak网站上的SPA测试应用程序。

打开 https://www.keycloak.org/app/。

单击保存以使用默认配置。

单击“登录”以使用之前启动的 Keycloak 服务器对此应用程序进行身份验证。

