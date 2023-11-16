# Tomcat

![Tomcat 架构](https://upload-images.jianshu.io/upload_images/28203940-3532e40a6922909d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## Connector

负责处理对外网络的 IO。Connector 对内屏蔽了不同的应用层协议和网络 IO 模型 。Tomcat 支持的网络 IO 模型有：

- BIO：同步阻塞式 IO
- NIO：同步非阻塞式 IO
- AIO(NIO2)：异步式 IO
- APR：操作系统层面处理异步式 IO

Connector 实现的功能：

- 监听网络端口，接受客户端网络连接请求
- 根据不同的应用层协议（HTTP1.1/HTTP2）解析数据流，封装成统一的 Request 对象
- 将 Tomcat Request 对象转换成标准的 ServletRequest 对象
- 将 ServletRequest 对象转交容器（Container），让容器处理业务任务，得到容器返回的 ServletResponse 对象
- 将 ServletResponse 对象转换成 Tomcat 统一的 Response 对象，然后序列化成字节流返回客户端。

Connector 组件：

- Endpoint ：负责提供数据字节流给 Processor
- Processor：提供 Tomcat Request 对象给 Adapter 
- Adapter：负责提供 ServletRequest 对象给 Container 处理业务

## Container

Tomcat基于分层架构的思想，设计了4种容器：Engine、Host、Context、Wrapper。它们之间是父子关系。

![](https://upload-images.jianshu.io/upload_images/28203940-c1a2208d5ca39f57.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### Engine

Engine 容器全局只有一个，是 Container 处理 Request 和 Response 的入口。Engine 会将请求路由给对应的 Host 容器。

### Host

Host 容器每个代表一个虚拟主机，对应不同的域名

### Context

通过一个 Context 标识一个应用，对应 wabapp 目录下的一个工程。Tomcat 应用 web.xml 文件就是由 Context 去解析的。

### Wrapper

每一个  Wrapper 对应一个 Servlet 实例，主要作用是载入 servlet 类实例化，并调用 service 方法。常见的 Spring 的 Dispatch Servlet 是线程安全的，所以 Tomcat 不需要保证  Servlet 的并发安全。