---
title: Netty 框架入门
date: 2023-05-18 18:40:24
tags: 
- Java
- Netty
---

## 优势

1. 统一API,多模型
2. 自带编解码器
3. 多通信协议
4. 高吞吐、低延迟、低资源消耗、少内存复制
5. 安全

## 核心组件

### Channel

Channel 接口是 Netty 对网络操作抽象类，它除了包括基本的 I/O 操作，如 bind()、connect()、read()、write() 等

### EventLoop

EventLoop 定义了 Netty 的核心抽象，用于处理连接的生命周期中所发生的事件

主要作用是负责监听网络事件并调用事件处理器进行相关 I/O 操作的处理。

### ChannelFuture

Netty 是异步非阻塞的，所有的 I/O 操作都为异步的。

我们不能立刻得到操作是否执行成功。
可以通过 ChannelFuture 接口的 addListener() 方法注册一个 ChannelFutureListener，
当操作执行成功或者失败时，监听就会自动触发返回结果。

另外，我们还可以通过 ChannelFuture 接口的 sync()方法让异步的操作变成同步的

### ChannelHandler 

```java
b.group(eventLoopGroup)
  .handler(new ChannelInitializer<SocketChannel>() {
    @Override
    protected void initChannel(SocketChannel ch) {
      ch.pipeline().addLast(new NettyKryoDecoder(kryoSerializer, RpcResponse.class));
      ch.pipeline().addLast(new NettyKryoEncoder(kryoSerializer, RpcRequest.class));
      ch.pipeline().addLast(new KryoClientHandler());
    }
  });
```

ChannelHandler 是消息的具体处理器。他负责处理读写操作、客户端连接等事情。

### ChannelPipeline

ChannelPipeline 为 ChannelHandler 的链，提供了一个容器并定义了用于沿着链传播入站和出站事件流的 API 。

当 Channel 被创建时，它会被自动地分配到它专属的ChannelPipeline。  
我们可以在 ChannelPipeline 上通过 addLast() 方法添加一个或者多个ChannelHandler ，
因为一个数据或者事件可能会被多个 Handler 处理。当一个 ChannelHandler 处理完之后就将数据交给下一个 ChannelHandler 。

### EventLoopGroup

EventLoopGroup 包含多个 EventLoop（每一个 EventLoop 通常内部包含一个线程），
EventLoop 的主要作用实际就是负责监听网络事件并调用事件处理器进行相关 I/O 操作的处理。
并且 EventLoop 处理的 I/O 事件都将在它专有的 Thread 上被处理，即 Thread 和 EventLoop 属于 1 : 1 的关系，从而保证线程安全。
Boss EventloopGroup 用于接收连接，
Worker EventloopGroup 用于具体的处理（消息的读写以及其他逻辑处理）。
当客户端通过 connect 方法连接服务端时，bossGroup 处理客户端连接请求。
当客户端处理完成后，会将这个连接提交给 workerGroup 来处理，然后 workerGroup 负责处理其 IO 相关操作。

### Bootstrap 

客户端的启动引导类/辅助类，具体使用方法如下： 

```java
// [1]. 创建一个 NioEventLoopGroup 对象实例
EventLoopGroup group = new NioEventLoopGroup();
try {
  // [2]. 创建客户端启动引导/辅助类：Bootstrap
  Bootstrap b = new Bootstrap();
  // [3]. 指定线程组
  b.group(group)
    // [4]. 指定 I/O 模型
    .channel(NioSocketChannel.class)
    .handler(new ChannelInitializer<SocketChannel>() {
        @Override
        public void initChannel(SocketChannel ch) throws Exception {
            ChannelPipeline cp = ch.pipeline();
            // [5]. 自定义消息的业务处理逻辑
            cp.addLast(new HelloClientHandler(message));
        }
    });
  // [6].尝试建立连接 connect
  ChannelFuture f = b.connect(host, port).sync();
  // [7].等待连接关闭（阻塞，直到Channel关闭）
  f.channel().closeFuture().sync();
} finally {
  // [8]. 优雅关闭相关线程组资源
  group.shutdownGracefully();
}
```
### ServerBootstrap 

服务端的启动引导类/辅助类，具体使用方法如下：       

```java
// [1] bossGroup 用于接收连接，workerGroup 用于具体的处理
EventLoopGroup bossGroup = new NioEventLoopGroup(1);
EventLoopGroup workerGroup = new NioEventLoopGroup();
try {
  //[2] 创建服务端启动引导/辅助类：ServerBootstrap
  ServerBootstrap b = new ServerBootstrap();
  //[3] 给引导类配置两大线程组,确定了线程模型
  b.group(bossGroup, workerGroup).
    // (非必备)打印日志
    .handler(new LoggingHandler(LogLevel.INFO))
    // [4] 指定 IO 模型
    .channel(NioServerSocketChannel.class)
    .childHandler(new ChannelInitializer<SocketChannel>() {
      @Override
      public void initChannel(SocketChannel ch) {
          ChannelPipeline p = ch.pipeline();
          // [5] 可以自定义客户端消息的业务处理逻辑
          p.addLast(new HelloServerHandler());
      }
    });
  // [6] 绑定端口,调用 sync 方法阻塞知道绑定完成
  ChannelFuture f = b.bind(port).sync();
  // [7] 阻塞到服务器Channel关闭(closeFuture()方法获取Channel的CloseFuture对象调用其sync()方法)
  f.channel().closeFuture().sync();
} finally {
  // [8] 优雅关闭相关线程组资源
  bossGroup.shutdownGracefully();
  workerGroup.shutdownGracefully();
}
```

Bootstrap 通常使用 **connet()** 方法连接到远程的主机和端口，作为一个 Netty TCP 协议通信中的客户端。  
另外，Bootstrap 也可以通过 **bind()** 方法绑定本地的一个端口，作为 UDP 协议通信中的一端。  
ServerBootstrap通常使用 **bind()** 方法绑定本地的端口上，然后等待客户端的连接。  
Bootstrap 只需要配置一个线程组 (EventLoopGroup) ,  
ServerBootstrap需要配置两个线程组 (EventLoopGroup)，一个用于接收连接，一个用于具体的处理。  

```java
ChannelFuture f = b.connect(host, port).addListener(future -> {
  if (future.isSuccess()) {
    System.out.println("连接成功!");
  } else {
    System.err.println("连接失败!");
  }
}).sync();
```

## Reactor 模型

Client
Acceptor 负责监听客户端的连接
Reactor
Selector
Dispatcher


## TCP 粘包/拆包

### what
TCP 粘包/拆包 就是你基于 TCP 发送数据的时候，出现了多个字符串“粘”在了一起或者一个字符串被“拆”开的问题

### how

使用 Netty 自带的解码器

1. LineBasedFrameDecoder : 发送端发送数据包的时候，每个数据包之间以换行符作为分隔  
2. DelimiterBasedFrameDecoder : 可以自定义分隔符解码器  
3. FixedLengthFrameDecoder: 固定长度解码器，它能够按照指定的长度对消息进行相应的拆包

自定义序列化编解码器在 Java 中自带的有实现 Serializable 接口来实现序列化，但由于它性能、安全性等原因一般情况下是不会被使用到的。
通常情况下，我们使用 Protostuff、Hessian2、json 序列方式比较多
