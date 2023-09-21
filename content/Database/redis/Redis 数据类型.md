# Redis 数据类型

Redis 有5种数据类型：

- String
- hash
- list
- set
- zset

## String

String 的值可以是：

- 简单字符串
- JSON、XML等
- 数字
- 二进制文件

String 内部编码：

- [[Database/redis/SDS]]（简单动态字符串）

String 使用场景：

- 缓存
- 计数
- 共享 Session
- 限速

## hash

hash 内部编码：

- [[Database/redis/ziplist]]（压缩列表）
- hashtable

hash 使用场景：

- 使用 key-value 模式存储用户信息

## list

列表中的元素是有序的，且元素可以重复。

list 内部编码：

- [[Database/redis/quicklist]]（快表）

list 使用场景：

- 消息队列
- 文章列表（分页数据）

## set


set 内部编码：

- intset（整数集合）
- hashtable

set 使用场景：

- 打标签

# zset

zset 底层有 [[Database/redis/ziplist]] 和 skiplist（[[跳表]]） 两种数据结构，两种结构间会相互转化。
当元素数量大于 128 或每个元素长度小于 64 字节的时候，会由 [[Database/redis/ziplist]] 转换为 skiplist。

如使用 ziplist，一个数据项存在两个相邻节点上，第一个节点保存 member，第二个节点保存 score。ziplist 内的集合元素按 score 从小到大排序。查找元素的时间复杂度为 O(n)。

![[Database/redis/attachments/Pasted image 20220411185630.png]]

如使用 skiplist，另外会维护一个字典。字典保存从 member 到 score 的映射，跳表按 score 从小到大保存所有集合元素，查找时间复杂度为平均 O(logN)，最坏 O(N) 。因为 zset 需要做范围查找，所以选用 skiplist 而不是哈希表或红黑树。

zset 使用场景：

- 排行榜