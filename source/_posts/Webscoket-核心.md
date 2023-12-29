---
title: Webscoket 核心
date: 2023-06-11 11:17:03
tags: 
- JavaScript
- WebSocket
cover:
---


# Webscoket 核心

## 是什么

一种服务端/浏览器端双向推送技术

## 为什么

因为 HTTP 协议有一个缺陷：通信只能由客户端发起

## 怎么用

构造函数（返回一个 WebSocket 对象）

```js
WebSocket(url[, protocols])
```

| Constant             | Value |
| -------------------- | ----- |
| WebSocket.CONNECTING | 0     |
| WebSocket.OPEN       | 1     |
| WebSocket.CLOSING    | 2     |
| WebSocket.CLOSED     | 3     |

| 属性           | 解释                                       |
| -------------- | ------------------------------------------ |
| binaryType     | 使用二进制的数据类型连接。                 |
| bufferedAmount | 只读 未发送至服务器的字节数。              |
| extensions     | 只读 服务器选择的扩展。                    |
| onclose        | 用于指定连接关闭后的回调函数。             |
| onerror        | 用于指定连接失败后的回调函数。             |
| onmessage      | 用于指定当从服务器接受到信息时的回调函数。 |
| onopen         | 用于指定连接成功后的回调函数。             |
| protocol       | 只读 服务器选择的下属协议。                |
| readyState     | 只读 当前的链接状态。                      |
| url            | 只读 WebSocket 的绝对路径。                |

| 方法                    | 解释                     |
| ----------------------- | ------------------------ |
| close([code[, reason]]) | 关闭当前链接。           |
| send(data)              | 对要传输的数据进行排队。 |

使用 addEventListener() 或将一个事件监听器赋值给本接口的 on<eventname> 属性，来监听下面的事件。

| 事件    | 解释                                                          |
| ------- | ------------------------------------------------------------- |
| close   | 当一个 WebSocket 连接被关闭时触发。                           |
| error   | 当一个 WebSocket 连接因错误而关闭时触发，例如无法发送数据时。 |
| message | 当通过 WebSocket 收到数据时触发。                             |
| open    | 当一个 WebSocket 连接成功时触发。                             |

### 客户端

```js
var ws = new WebSocket("wss://echo.websocket.org");

ws.onopen = function(evt) { 
  console.log("Connection open ..."); 
  ws.send("Hello WebSockets!");
};

ws.onmessage = function(evt) {
  console.log( "Received Message: " + evt.data);
  ws.close();
};

ws.onclose = function(evt) {
  console.log("Connection closed.");
};      
```

### 服务端


## STOMP

WebSocket协议定义了两种类型的消息，即文本和二进制，但它们的内容却没有定义。该协议定义了一种机制，供客户端和服务器协商子协议--即更高级别的消息传输协议，在WebSocket之上使用，以定义各自可以发送何种消息、每种消息的格式和内容，等等。子协议的使用是可选的，但无论如何，客户和服务器都需要就定义消息内容的一些协议达成一致。

STOMP是一个基于帧的协议，其帧是以HTTP为模型。一个STOMP帧的结构：

    COMMAND
    header1:value1
    header2:value2

    Body^@

## 参考资料

[WebSocket - Web API 接口参考 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/WebSocket)

[HTML5 WebSocket | 菜鸟教程](https://www.runoob.com/html/html5-websocket.html)

[WebSocket 教程 - 阮一峰的网络日志](http://ruanyifeng.com/blog/2017/05/websocket.html)

[WebSocket - 廖雪峰的官方网站](https://www.liaoxuefeng.com/wiki/1022910821149312/1103303693824096)