# NIO

NIO（Non-blocking I/O）指非阻塞 IO（*同步*），与BIO（Blocking I/O）相对应。NIO 是一种同步非阻塞的 [[IO|I/O]] 模型，也是  [[IO|I/O]] 多路复用的基础。

NIO 使用 Native 函数库直接分配堆外内存，然后通过一个存储在 Java 堆中的 `DirectByteBuffer`对象，对堆外内存进行直接引用。

NIO 适用于*连接数目多且连接比较短*的架构，如聊天服务器，弹幕系统，服务器间通讯等。

![[Pasted image 20220713192133.png]]

## NIO 与 BIO 的区别

1. BIO 以**流**的方式处理数据，NIO 以**块**的方式处理数据，块方式的效率要高于流。
2. BIO 是阻塞的，NIO 是非阻塞的。
3. BIO 基于字节流和字符流进行操作，NIO 基于 Channel（通道）和 Buffer（缓冲区）进行操作，数据总是从通道读取到缓冲区中，或者从缓冲区写入到通道中。Selector（选择器）用于监听各个通道，因此使用**单个线程**就可以监听多个客户端通道。
4. BIO 是单向的，如：InputStream，OutputStream；而 NIO 是双向的，既可以用来进行读操作，又可以用来进行写操作。

## Buffer

Buffer（缓冲区）是一个用于存储特定基本类型数据的容器，本质上是一块可以写入和读出数据的内存区域。每种基本类型都有一个对应的buffer类（除boolean）：

- ByteBuffer
- CharBuffer
- DoubleBuffer
- FloatBuffer
- IntBuffer
- LongBuffer
- ShortBuffer

Buffer 有三个主要属性：`capacity`（容量），`position`（游标），`limit`（末尾限定符）

## Channel

Channel（通道）表示到实体，如硬件设备、文件、网络套接字或可以执行一个或多个不同 I/O 操作（如读取或写入）的程序组件的开放连接。

Channel 接口的常用实现类有：

- `FileChannel`（对应文件IO）
- `DatagramChannel`（对应UDP）
- `SocketChannel` 和 `ServerSocketChannel`（对应 TCP 的客户端和服务器端）

Channel 和 BIO 中的 Stream（流）是差不多一个等级的。

## Selector

Selector（选择器）用于监听多个 Channel（通道）的事件（比如：连接打开，数据到达）。因此，单个的线程可以监听多个数据通道。即用选择器，借助单一线程，就可对数量庞大的活动 I/O 通道实施监控和维护。

## NIO 编程过程分析

1.  当客户端连接时，会通过 `SeverSocketChannel` 得到对应的 `SocketChannel`。
2.  Selector 进行监听，调用 `select()`方法，返回注册该 `Selector` 的所有通道中有事件发生的通道个数。
3.  将 socketChannel 注册到 Selector 上，`public final SelectionKey register(Selector sel, int ops)`，一个 selector 上可以注册多个 SocketChannel。
4.  注册后返回一个 SelectionKey，会和该 Selector 关联(以**集合**的形式)。
5.  进一步得到各个 SelectionKey。
6.  有事件发生，再通过 SelectionKey 反向获取 SocketChannel，使用 `channnel()`方法。
7.  可以通过得到的 channel，完成业务处理。