# LFU(Least frequently used)

LFU，**最不经常使用**算法，是一种缓存淘汰算法，淘汰一定时期内访问次数最少的数据。

## LFU 数据结构实现

LFU 有两种实现方式，一是哈希表 + 平衡二叉树，二是双哈希表。以下以双哈希表的方案为例。

- 双哈希表方案需要维护两个哈希表以及一个最少频次使用计数 minFreq。
- 第一个哈希表是 freq_table，key 存储访问频次，value 是一个[[数据结构&算法/链表#双向链表|双向链表]]，双向链表的每一个节点包含3个元素：key， value 以及 freq。
- 第二个哈希表是 key_table，key 为双向链表中存储的 key，value 是对应的链表节点的指针。

[LFU双哈希表实现](https://www.cnblogs.com/xiaoxiaolinux/p/15306802.html)
