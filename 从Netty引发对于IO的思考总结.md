# 从Netty引发对于IO的思考总结

在学习dubbo的底层原理时，发现了其通信原理是基于Netty的，于是乎本着技多不压身的原则，花了一个星期的业余时间系统的了解了一下Netty,对于IO也有了一个更深层次的理解。（我之前对于IO还停留在输入输出流的层面，实属惭愧）

#### Netty简介

Netty是一个异步的基于事件驱动的网络应用框架，用以快速开发高性能，针对在TCP协议下面向Client端的高并发应用（NIO框架）

#### IO模型介绍

BIO 同步阻塞，一个连接一个线程，适用于连接数目比较小且固定的架构

NIO 同步非阻塞，一个线程处理多个请求，客户端发送的请求都会注册到多路复用器，轮询处理，适用于连接数目多且时间短的架构

AIO 异步非阻塞，引入了异步通道的概念，简化了程序的编写，适用于连接数目多且连接时间比较长的情况

下面重点总结NIO

NIO的三大模块 channel buffer selector

Netty是基于主从Reactor多线程模型，并且做了一定的改进

#### Netty的核心组件

BootStrap 客户端引导 

Server BootStrap 服务器端引导

Channel 通信组件，用于执行网络IO操作，可获得当前网络连接的通道的状态，获得网络连接的配置参数，提供异步的网络IO操作，调用立即返回一个Channel Future实例

Selector 实现IO多路复用，当向一个selector中注册多个channel后，selector内部的机制就可以自动不断的查询这些注册的channel是否有已就绪的IO事件（eg.可读，可写，网络连接）等，这样程序就可以简单使用一个线程高效的管理多个channel

ChannelHandler 接口，处理IO事件，拦截IO

Pipeline 双向链表，维护多个Handler

ChannelHandlerContext 保存了channel相关的所有上下文信息，同时关联了一个channelHandler对象

Unpooled工具类 Netty提供了一个专门用来操作缓冲区的工具类

#### 小结 

Netty抽象出2组线程池，BossGroup专门负责接收客户端的连接，WorkGroup专门负责网络的读写

BossGroup和WorkGroup的类型都是NioEventLoopGroup,含有多个事件循环，每一个事件循环都是NIOEventLoop

NIOEventLoop表示一个不断循环的执行处理任务的线程，每个NIOEventLoop都有一个Selector,用于监听绑定在其上的socket网络通讯

NIOEventLoopGroup可以有多个线程，即可以含有多个NIOEventLoop

每个BossNIoEventLoop的循环执行的步骤有3步

1 轮询accept事件

2 处理accep事件，与client建立连接，生成NIOSocketChannel并将其注册到某个WorkNioEventLoop上的Selector

3 处理任务队列的任务，即Run All Tasks

每个WorkNIOEventLoop循环执行的步骤

1 轮询read,write事件

2 处理I/O,在对应的NIOSocketChannel处理

3 处理任务队列的任务，即Run All Tasks





