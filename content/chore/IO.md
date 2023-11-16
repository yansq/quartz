# I/O

IO是指Input/Output，即输入和输出，**以内存为中心**：
- Input指从外部读入数据到内存，例如把文件从磁盘读取到内存，从网络读取数据到内存等等
- Ouput指把数据从内存输出到外部，例如把数据从内存写入到文件，把数据从内存输出到网络等等

IO是一种顺序读写数据的模式，它的特点是单向流动，数据类似自来水一样在水管中流动，所以称为IO流。
IO流以`byte`（字节）为最小单位。

在Java中，[[InputStream]]代表输入字节流，`OutputStream`代表输出字节流。
在Java中，除以上两种 IO 流之外，还提供了`Reader`和`Writer`的字符流，字符流传输的最小数据单位是`char`，`Reader`和`Writer`本质上是能**自动编解码**的`InputStream`和`OutputStream`，这里的编解码可以理解为修改字符编码格式。

记忆点：以 Stream 结尾的都是字节流，以 Reader 和 Writer 结尾的都是字符流。