# Kafka 面试题

## consumer 是推还是拉？

拉（pull）。在异构架构中，能让不同消费能力的 consumers 都达到最大的消费速率。

## Kafka 与传统 MQ 消息系统之间有什么区别

- 一条消息可以被重复消费（重置 offset），消息持化
- 分布式系统，动态扩缩容
- 提供流处理

## 如何减少消息丢失

1.  不要使用 producer.send(msg)，而要使用 producer.send(msg, callback)，带有回调通知的 send 方法。
2.  设置 acks = all。acks 是 Producer 的一个参数，代表了你对“已提交”消息的定义。如果设置成 all，则表明所有副本 Broker 都要接收到消息，该消息才算是“已提交”。这是最高等级的“已提交”定义。
3.  设置 retries 为一个较大的值。这里的 retries 同样是 Producer 的参数，对应前面提到的 Producer 自动重试。当出现网络的瞬时抖动时，消息发送可能会失败，此时配置了 retries > 0 的 Producer 能够自动重试消息发送，避免消息丢失。
4.  设置 unclean.leader.election.enable = false。这是 Broker 端的参数，它控制的是哪些 Broker 有资格竞选分区的 Leader。如果一个 Broker 落后原先的 Leader 太多，那么它一旦成为新的 Leader，必然会造成消息的丢失。故一般都要将该参数设置成 false，即不允许这种情况的发生。
5.  设置 replication.factor >= 3。这也是 Broker 端的参数。其实这里想表述的是，最好将消息多保存几份，毕竟目前防止消息丢失的主要机制就是冗余。
6.  设置 min.insync.replicas > 1。这依然是 Broker 端参数，控制的是消息至少要被写入到多少个副本才算是“已提交”。设置成大于 1 可以提升消息持久性。在实际环境中千万不要使用默认值 1。
7.  确保 replication.factor > min.insync.replicas。如果两者相等，那么只要有一个副本挂机，整个分区就无法正常工作了。我们不仅要改善消息的持久性，防止数据丢失，还要在不降低可用性的基础上完成。推荐设置成 replication.factor = min.insync.replicas + 1。
8.  确保消息消费完成再提交。Consumer 端有个参数 enable.auto.commit，最好把它设置成 false，并采用手动提交位移的方式。就像前面说的，这对于单 Consumer 多线程处理的场景而言是至关重要的。

## Kafka 吞吐量为何高

- offset 磁盘顺序读写
- 零拷贝（从pagecache（操作系统内存）直接到网络，不经过用户态）
- 分区
- 索引文件（索引放在内存）
- 批量读写
- 批量压缩

## **min.insync.replicas**

- 0: 不等待确认直接认为成功
- 1: Leader 确认后认为成功
- All: 所有副本(Leader Followers)确认后认为成功
- 
