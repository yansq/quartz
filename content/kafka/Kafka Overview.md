# Kafka 

## Message Format

Kafka uses binary steam to pass the messages.

## Transformation Patterns

- Peer to Peer (P2P）
- Publish-subscribe pattern

## Partition

Each topic can be divided into several partitions. Messages sent by producers can only arrive at exactly one partition of the topic.

## Offset

Offset is a simple integer number that is used by Kafka to maintain the current position of a consumer.

### Partitioning strategy

- Round-robin (default strategy)
- Randomness
- Key-ordering

## Replica

- Leader Replica：对外提供服务
- Follower Replica：不与外界交互

每个 Partition 的副本中只能有一个 Leader Replica，其他的都是 Follower Replica。

工作机制：生产者向 Leader Replica 写消息。Follower Replica 向 Leader Replica 发送请求，拉取消息，保持同步。

## Persistence

Kafka utilizes log files to store data on disk. The data can only be written sequentially, which leads to high write performance.

Log 分为多个日志段（Log Segment），由定时任务来定期清理过期的 Log Segment，释放硬盘空间。

## Consumer Group

Consumer Group 是 Kafka 对于 P2P 模式的实现。Topic 中的每个 Partition 只会被 Consumer Group 中的一个 Consumer Instance 消费到。

老版本的 Consumer Group 把位移保存在 ZooKeeper 中。Apache ZooKeeper 是一个分布式的协调服务框架，Kafka 重度依赖它实现各种各样的协调管理。将位移保存在 ZooKeeper 外部系统的做法，最显而易见的好处就是减少了 Kafka Broker 端的状态保存开销。现在比较流行的提法是将服务器节点做成无状态的，这样可以自由地扩缩容，实现超强的伸缩性。Kafka 最开始也是基于这样的考虑，才将 Consumer Group 位移保存在独立于 Kafka 集群之外的框架中。

不过，慢慢地人们发现了一个问题，即 ZooKeeper 这类元框架其实并不适合进行频繁的写更新，而 Consumer Group 的位移更新却是一个非常频繁的操作。这种大吞吐量的写操作会极大地拖慢 ZooKeeper 集群的性能，因此 Kafka 社区渐渐有了这样的共识：将 Consumer 位移保存在 ZooKeeper 中是不合适的做法。

于是，在新版本的 Consumer Group 中，Kafka 社区重新设计了 Consumer Group 的位移管理方式，采用了将位移保存在 Kafka 内部主题的方法。这个内部主题就是让人既爱又恨的 __consumer_offsets。

## rebalance 

Rebalance 就是让一个 Consumer Group 下所有的 Consumer 实例就如何消费订阅主题的所有分区达成共识的过程。

rebalance will happen when
- number of consumers in the consumer group changes
    - heartbeat failed
    - consumer's dealing time is too long
- number of subscribing topics  changes
- number of partitions of subscribing topics changes

## ZooKeeper

保存 Kafka 集群的元数据信息，如有哪些 Broker，有哪些 Topic，每个 Topic 的分区数，分区的副本在哪些机器上等。

## Configurations

Kafka 参数分为 Broker 参数与 Topic 参数。Broker 参数在配置文件中配置，Topic 参数使用 `kafka-configs`命令配置。参数冲突时，Topic 参数覆盖 Broker 参数。

JVM 参数：业界公认采用 6GB 的堆内存。
GC：推荐使用 G1。

## Zero Copy



为防止 producer 端无故丢失消息，不要使用 producer.send(msg)，而要使用 producer.send(msg, callback)。记住，一定要使用带有回调通知的 send 方法。

**如果是多线程异步处理消费消息，Consumer 程序不要开启自动提交位移，而是要应用程序手动提交位移**。在这里我要提醒你一下，单个 Consumer 程序使用多线程来消费消息说起来容易，写成代码却异常困难，因为你很难正确地处理位移的更新，也就是说避免无消费消息丢失很简单，但极易出现消息被消费了多次的情况。

-   至少一次（at least once）：消息不会丢失，但有可能被重复发送。

## 幂等性 Producer

enable.idempotence = true
只能保证单分区、单会话（在一个Producer进程中）的幂等性

## 事务型 Producer

change producer's config:

`enable.idempotence = true`
设置 Producer 端参数 `transctional. id`

then change consumer's config:
`isolation.level = read_committed (read_uncommitted is the default)``


在 Rebalance 过程中，所有 Consumer 实例都会停止消费，等待 Rebalance 完成。这是 Rebalance 为人诟病的一个方面。

## Offsets Topic

Offsets Topic is an inner topic in Kafka called "\__consumer_offsets". The "\__consumer_offsets" topic stores the offsets data of consumers. 

The data structure is a key-value pair. 
- Key contains 3 parts: **<Group ID，Topic Name，Partition Number >**.
- Value is the offset and some meta-data like timestamp.

## 主题删除失败

原因：待删除主题在执行迁移

解决方法：
1. 手动删除 ZooKeeper 节点 /admin/delete_topics 下以待删除主题为名的 znode。
2.  手动删除该主题在磁盘上的分区目录。
3. 在 ZooKeeper 中执行 rmr /controller，触发 Controller 重选举，刷新 Controller 缓存。

## coordinator

消费者组的协调者，rebalance 的主导者

## controller

broker 之一，用于与zookeeper交互，处理元数据

---

[References](https://learn.lianglianglee.com/%E4%B8%93%E6%A0%8F/Kafka%E6%A0%B8%E5%BF%83%E6%8A%80%E6%9C%AF%E4%B8%8E%E5%AE%9E%E6%88%98/00%20%E5%BC%80%E7%AF%87%E8%AF%8D%20%20%E4%B8%BA%E4%BB%80%E4%B9%88%E8%A6%81%E5%AD%A6%E4%B9%A0Kafka%EF%BC%9F.md)

