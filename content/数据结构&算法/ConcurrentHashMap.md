

#TODO 

## 线程安全设计

`ConcurrentHashMap` 是线程安全的，其锁设计在 JDK 1.8 前后有所区分。

### JDK 1.7 及之前：

使用**分段锁**设计，整体结构是一个 segment 数组，每一个 segment 代表哈希区间中的一部分。segment 指向一个传统 HashEntry 的数组 + 链表（JDK1.7还没有红黑树）的数据结构。

因此向这个数据结构写入数据的时候需要进行两次 hash 操作，第一次找到所属 segment，第二次找到 segemnt 中数组的对应位置。

![[Pasted image 20221128090938.png]]

在进行并发操作时，使用 [[ReentrantLock]]锁住对应的 segment。

### JDK1.8 起

JDK1.8 起，`ConcurrentHashMap` 放弃使用了段结构，其数据结构与传统 HashMap 或是 HashTable 类似。锁结构使用  [[synchronized]] 加 [[CAS]] 的方式。不发生哈希冲突时，使用 CAS；发生哈希冲突时，使用 Synchronized 加锁，只锁当前[[数据结构&算法/链表]]或[[红黑树]]的头节点。

