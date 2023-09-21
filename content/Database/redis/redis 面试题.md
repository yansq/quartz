# redis 面试题

## 什么是redis，为什么要使用它

redis 是一个 key-value 的内存数据库，由于使用内存，所以它的读写速度都会大大快于传统数据库。并且由于 redis 具有原子性，因此可运用此特性实现分布式系统的一些功能。

## redis一般有哪些使用场景

- 缓存
- 排行榜
- 计数器（流量限制）
- 消息队列
- 限时业务的运用（优惠券等）
- 分布式锁（setnx）
- 延时操作

## redis 有哪些数据类型

5种基础数据类型：

- String
- Hash
- List
- Set
- Zset

3种特殊数据类型：

- HyperLogLogs
- Bitmap
- geospatial

## 谈谈redis的对象机制（redisObject)

```C
/*
 * Redis 对象
 */
typedef struct redisObject {

    // 类型
    unsigned type:4;

    // 编码方式
    unsigned encoding:4;

    // LRU - 24位, 记录最末一次访问时间（相对于lru_clock）; 或者 LFU（最少使用的数据：8位频率，16位访问时间）
    unsigned lru:LRU_BITS; // LRU_BITS: 24

    // 引用计数
    int refcount;

    // 指向底层数据结构实例
    void *ptr;

} robj;

```

- type：记录对象类型，5种基础类型之一
- encoding：底层实现的数据结构
- lru：最近一次访问时间
- refcount：引用计数

当执行一个处理数据类型命令的时候，redis 执行以下步骤：

- 根据给定的 key，在数据库字典中查找和他相对应的 redisObject，如果没找到，就返回 NULL；
- 检查 redisObject 的 type 属性和执行命令所需的类型是否相符，如果不相符，返回类型错误；
- 根据 redisObject 的 encoding 属性所指定的编码，选择合适的操作函数来处理底层的数据结构；
- 返回数据结构的操作结果作为命令的返回值。

## redis数据类型有哪些底层数据结构

![redis 数据结构](https://tva1.sinaimg.cn/large/e6c9d24ely1h102lrn74hj21fk0u0469.jpg)

## 为什么要设计 sds（简单动态字符串）

Redis 没有使用C语言的默认类型，而是自己实现了一个。

sds 包括3个部分：头部，数据部，以及末尾的一个`\0`。

![sds 结构](https://tva1.sinaimg.cn/large/e6c9d24ely1h103i33ptrj20eo01nmwz.jpg)



-   一个字符串类型的值能存储最大容量是多少？512M
-   为什么会设计Stream
-   Stream用在什么样场景
-   消息ID的设计是否考虑了时间回拨的问题

