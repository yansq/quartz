# Transaction Log

为了保证存储在内存中的数据不丢失，ES的每个[[Database/ES/分片]]默认有一个 Transaction Log文件。

在[[Database/ES/文档（document）]]index时，数据同时会写入Transaction Log中。

在[[Database/ES/刷新（refresh）]]操作中，Index Buffer被清空，Transaction Log不会被清空。
在[[Database/ES/flush]]操作中，Transaction Log会被清空。
