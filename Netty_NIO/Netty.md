---
typora-copy-images-to: images
---

# Netty + NIO



## 1 NIO



### 1.1 BIO/NIO/AIO

|      | BIO        | NIO          | AIO          |
| ---- | ---------- | ------------ | ------------ |
| 模式 | 同步阻塞式 | 同步非阻塞式 | 异步非阻塞式 |
| 并发 | 差         | 好           | 好           |
|      |            |              |              |



### 1.2 缓冲区Buffer



类型化Buffer

只读Buffer

内存中直接修改的Buffer

### 1.3 通道Channel



### 1.4 选择器Selector

能够检测多个注册的通道是否有事件发生，只有在通道有事件时才进行读写，大大减少开销，且不必频繁切换线程上下文

![](C:\Users\yanglin\Downloads\Selector.png)

#### 1.4.1 获得一个selector

#### 1.4.2 Selector的方法

#### 1.4.3 NIO的通讯原理

#### 1.4.4 NIO实例1：非阻塞通信

#### 1.4.5 SelectionKey API

- 四种事件



#### 1.4.6 ServerSocketChannel

- open
- bind
- configBlocking
- accept
- register



#### 1.4.7 SocketChannel



#### 1.4.8 NIO实例2：聊天室



### 1.5 零拷贝



#### 1.5.1 零拷贝的概念

- CPU Copy和DMA Copy
- 零拷贝：不进行CPU拷贝，没有重复数据产生

#### 1.5.2 零拷贝的实现

- mmap内存映射：将文件映射到内存缓冲区，用户空间可以共享内核缓冲区的内容，减少用户空间拷贝
  - 
- sendFile
  - 1.0
  - 2.0（Linux 2.4）：实现了真正的零拷贝，只有少量的信息需要进行CPU拷贝
- NIO：transferTo方法
  - Windows：分段传输，每次最大8M
  - Linux：无限制



## 2 Netty

异步的，基于事件驱动的网络应用框架

### 2.1 概述

### 2.2 Netty的高性能架构

#### 2.2.1 传统的阻塞IO模型

- 存在的问题：
  - **线程多**，占用很大的系统资源，不适用于并发数大的场景
  - 存在**资源浪费**的情况，当线程连接的客户端没有数据传输时会阻塞

#### 2.2.2 Reactor模式

- 实现Reactor模式的技术
  - IO复用模型
  - 线程池机制

- 三种Reactor模式

  - 单Reactor单线程模式

  - 单Reactor多线程模式

     ![Reactor模式2](D:\DeveloperStudy\Notes\Netty_NIO\images\Reactor模式2.png)

    - 优点：提升了利用CPU的效率
    - 缺点1：数据共享和访问比较复杂
    - 缺点2：Reactor承担所有事件的监听和响应，单线程运行，在高并发场景容易出现性能瓶颈

  - 主从Reactor多线程模式

    ![Reactor模式3](D:\DeveloperStudy\Notes\Netty_NIO\images\Reactor模式3.png)

    - 处理流程：
      - Reactor主线程通过select监听连接事件，收到事件后，通过Acceptor处理连接事件
      - 当Acceptor处理连接事件后，主Reactor将连接分配给从Reactor
      - 从reactor将连接加入连接队列进行监听，并创建handler进行事件处理
      - 有新事件发生时，从reactor调用相应的handler进行处理
      - handler通过read读取数据，分发给后面的worker线程处理
      - worker线程池分配独立的worker线程进行业务处理，返回结果给handler
      - handler收到结果，通过send将结果返回给客户端
      - 主Reactor只处理连接请求，其他请求分配给从Reactor分发
    - 优点：职责明确，父子reactor之间数据交互简单
    - 缺点：编程复杂度高

- 优点：
  - 响应快，但需要注意Reactor本身是同步的
  - 最大程度地避免复杂的多线程和同步问题，避免多线程/多进程之间的切换开销
  - 扩展性好，可以方便地增加Reactor实例数量充分利用CPU资源
  - 复用性好，模式本身与具体的业务逻辑无关

#### 2.2.3 Netty模型

基于主从reactor多线程模型的改进，多主reactor

![netty模型](D:\DeveloperStudy\Notes\Netty_NIO\images\netty模型.png)

- BossGroup维护selector，只关注Accept事件



### 2.3 Netty异步模型

2.3.1 异步处理请求的三种方式

- taskQueue
- scheduledTaskQueue
- 非当前Reactor线程调用Channel里的各种方法
  - 获得用户标识，使用集合管理用户的channel，当需要推送消息时，可以将业务交给各个channel对应的NioEventLoop的taskQueue中

2.3.2 Future-Listener机制



### 2.4 Netty核心组件

#### 2.4.1 BootStrap和ServerBootStrap

配置netty程序，串联各个组件，做相关配置

- group()
  - 两个参数：用于服务器端，设置两个group
  - 一个参数：用于客户端，设置一个group
- channel()
  - 设置一个服务器端的通道类型（传入类型参数）
- option()
  - 给ServerChannel添加配置
- childOption()
  - 给接收的通道添加配置（工作组）
- childHandler()
  - 设置业务处理类（Handler）
- bind()
  - 用于服务器端绑定端口
- connect()
  - 用于客户端连接服务器

#### 2.4.2 Future和ChannelFuture

用来实现异步IO操作，监听事件的处理结果，实现函数回调

- channel()
  - 返回当前正在进行IO操作的通道
- sync()
  - 等待当前异步操作执行完毕

#### 2.4.3 Channel

- netty网络通信的组件，用于执行网络IO操作
- 不同的协议，不同的阻塞类型都有不同的Channel类型（TCP/UDP/SCTP）
- 通过Channel可以获得当前网络连接的各种信息，如网络连接的配置参数，缓冲区大小等
- 可以提供异步的网络IO
- 调用后将立即返回一个ChannelFuture实例，可以注册监听器到Cf上实现异步回调

#### 2.4.4 Selector

实现IO多路复用，通过一个selector线程监听多个channel的事件

#### 2.4.5 ChannelHandler及实现类

- 常见的方法：通道就绪事件、通道读取数据事件、通道读取完毕事件等

- 入站ChannelInboundHandler和出站ChannelOutboundHandler
  - 从管道读，称为入站，向管道写，称为出站（服务器和客户端都是如此）

#### 2.4.6 Pipeline和ChannelPipeline

![Channel和ChannelPipeline](D:\DeveloperStudy\Notes\Netty_NIO\images\Channel和ChannelPipeline.png)

- addFirst()和addLast()：添加Handler

#### 2.4.7 ChannelHandlerContext（接口）

含有Channel所有的上下文信息，包括事件处理器Handler，同时绑定了对应的pipeline和channel

#### 2.4.8 ChannelOption

是Channel实例的一个配置参数

- ChannelOption.SO_BACKLOG：TCP/IP协议中的listen函数中的backlog参数，初始化服务器可连接队列的大小
- ChannelOption.SO_KEEPALIVE：保持活动链接状态

#### 2.4.9 EventLoop和EventLoopGroup

EventLoopGroup是一组EventLoop的抽象，为更好利用多核CPU资源，多个EventLoop同时工作，每个EventLoop维护一个Selector

- shutdownGracefully()：断开连接，关闭线程

#### 2.4.10 Unpooled和ByeBuf

Unpooled：netty专门操作缓冲区（ByteBuf）的工具类

- Unpooled.buffer(int capacity)：创建ByteBuf对象，内部为byte数组，长度为capacity
- Unpooled.copiedBuffer(data, 编码方式)：创建ByteBuf，容量大于实际数据的大小

ByteBuf：缓冲区的数据类型，不需要flip，维护了两个索引readerIndex和writerIndex以及容量capacity

**0~reader：已读取的废弃区域**

**reader~writer：可读区域**

**writer~capacity：可写区域**

- readByte()：每次读readerIndex对应的数据，每次读之后readerIndex++
- getByte(idx)：读指定索引的数据，不变化readerIndex
- hasArray()：检查buf中有无分配的数组
- array()：返回buf中的数组
- arrayOffset()：数组偏移
- readerIndex()：读索引位置
- readableBytes()：可读字节数
- writerIndex()：写索引位置
- capacity()：容量
- getCharSequence(start, length, 字符集)：返回一段数据



### 2.5 Netty实例1：群聊系统



### 2.6 Netty实例2：心跳机制

当指定时间没有读/写操作时，提示读/写空闲



### 2.7 Netty实例3：WebSocket长连接



### 2.8 Google ProtoBuf

#### 2.8.1 编码与解码

codec--编解码器（encoder--编码器，decoder --解码器）

Netty提供的codec：

- 底层仍然使用JAVA序列化，不能跨语言传输
- 序列化后体积比较大，性能比较低

#### 2.8.2 概述

- Google ProtoBuf解决上述问题：**轻便高效的结构化数据存储格式**
- 很适合数据存储或**RPC远程过程调用**
- **转型：http+json ---- tcp+protobuf**
- 以message方式管理
- 跨语言、跨平台
- 高性能、高可靠性
- 使用.proto文件编写
- protoc.exe可以自动转为.java



### 2.9 Netty的编解码器

![入站出站](D:\DeveloperStudy\Notes\Netty_NIO\images\入站出站.png)



### 2.10 TCP粘包和拆包

发送端为了更有效的传输数据，将多个包发送给对方，使用Nagle算法优化，将**多个数据量较小且间隔较小的数据合成数据块进行封包**（且TCP无消息保护边界）。

![TCP粘包拆包](D:\DeveloperStudy\Notes\Netty_NIO\images\TCP粘包拆包.png)

解决方法：**自定义数据类型，在自定义数据类型中提供数据长度**