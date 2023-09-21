# ZooKeeper

 ZooKeeper 是一个分布式的，开放源码的分布式应用程序协调服务，是 Google Chubby 的一个开源的实现，是 Hadoop 和 Hbase 的重要组件。ZooKeeper 基于观察者模式，适合**读多写少**的场景。集群中每个服务节点都是将数据全量存储在内存中，适用于**轻量级数据**的应用场景。

 它是一个为分布式应用提供一致性服务的软件，提供的功能包括：
 
 - 分布式消息同步和协调机制
- 服务器节点动态上下线
- 统一配置管理
- 负载均衡
- 集群管理

ZooKeeper 使用 `ZAB` 协议进行**消息广播**，**崩溃恢复**和**数据同步**

## 数据结构

ZooKeeper 数据模型的结构与 Unix 文件系统类似，整体上看是一颗目录树，每一个节点称为 `ZNode`（每个节点不但有目录名称，还必须要有值，类似于键值对）,每个 `ZNode` 默认能存储 1MB 的数据，且每个 `ZNode` 可以通过路径唯一标识。

`ZNode`共有4种类型：

- 短暂（EPHEMERAL）：客户端和服务器端断开连接后，创建的节点自己删除；
- 持久（PERSISTENT）：客户端和服务器端断开连接后，创建的节点不删除；
- 持久化顺序编号目录节点（PERSISTENT_SEQUENTIAL）：客户端与 ZooKeeper断开连接后，该节点依旧存在，只是 ZooKeeper 给该节点名称进行顺序编号，顺序编号有小到大；
- 临时顺序编号目录节点（EPHEMERAL_SEQUENTIAL）：客户端与 ZooKeeper 断开连接后，该节点被删除，只是 ZooKeeper 给该节点名称进行顺序编号，顺序编号有小到大。

`ZNode`由3个部分组成：
- stat：状态信息，描述 `ZNode` 的版本与权限
- data：存储相关数据
- children：`ZNode`的子节点

## Watcher（事件监听器）

ZooKeeper 允许用户在指定节点上注册一些 Watcher，并且在一些特定事件触发的时候，ZooKeeper 服务端会将事件通知到感兴趣的客户端上去。该机制是 ZooKeeper 实现分布式协调服务的重要特性。

- 父节点的创建，修改，删除都会触发 Watcher 事件。
- 子节点的创建，删除会触发 Watcher 事件。

ZooKeeper 中的 Watcher 是**一次性**的，即触发一次就会被取消，如果想继续Watch的话，需要客户端重新设置 Watcher。

## 集群

ZooKeeper 集群有 1 个 Leader，若干 Follower ，若干 Observer 组成：

- Leader：负责投票的发起和决议，更新系统状态和数据，保证事务处理的顺序性
- Follower：用于接收客户请求并向客户端返回结果，在选举 Leader 过程中参与投票，follower 没有权限更新数据，只能读数据。
- Observer：独立处理非事务请求，对于事务请求，转发给 Leader 服务器处理，不参与投票

ZooKeeper 的每个 server 都保存一份相同的数据副本，client 无论连接到哪个server，数据都是一致的。

更新请求顺序进行，来自同一个 clien t的更新请求按其发送顺序依次执行，相当于一个队列，先进先出。

数据更新原子性，一次数据更新要么成功，要么失败。

ZooKeeper 符合[[CAP理论]]中的 `CP`，在选举期间无法对外提供服务。

## 写操作

1. Client 发送一个写请求
2. 如果由 Follower 接收到写请求，将该请求转发到 Leader
3. Leader 收到请求后发起投票
4. Follower 将投票持久化到磁盘上，然后将投票结果发送给 Leader
5. 如果收集到半数以上票，Leader commit 本地事务，并发送 commit 通知所有 Follower
6. Follower 各自完成事务提交
7. Leader 节点将最新数据同步给 Observer 节点
8. 返回结果给 client

## 选举

半数机制：集群中半数以上机器存活，集群可用。

ZooKeeper 没有指定 leader 和 follower，均由内部选举机制产生。

选举过程：

- 假如有5台服务器，编号分别为server1~5,
- 当 server1 启动时，它向配置中的其他 server 发出请求，但是其他服务器没有启动，没有回应，处在 looking 状态。
- 当 server2 启动时，开始与 server1 进行交流，开始选举。它们首先分别给自己一票，但是这样没法选举出 leader；所以服务器号数低的向高的投票，这时候server2 有两票，但是没有超过半数，继续处在 looking 状态。
- 当 server3 启动时，与 server1 和 server2 进行选举，最终 server3 获得3票，超过半数，成为 leader。
- 当 server4 启动时，与前面服务器交换数据得到3就是 leader，那自己就当follower了。
- server5 情况和 server4 相同。

## ZooKeeper 实现分布式锁

- 业务线程 1，业务线程 2 分别向 ZK 的 `/lock` 目录下，申请创建有序的**临时节点**。
- 业务线程 1 抢到 `/lock0001` 的文件，也就是在整个目录下最小序的节点，也就是线程 1 获取到了锁。
- 业务线程 2 只能抢到 `/lock0002` 的文件，并不是最小序的节点，线程 2 未能获取锁。
- 业务线程 1 与 `lock0001` 建立了连接，并维持了心跳，维持的心跳也就是这把锁的租期。
- 当业务线程1 完成了业务，将释放掉与 ZK 的连接，也就是释放了这把锁。

与 Redis 分布式锁的区别：
- ZK 锁是 `CP` 锁，可靠性强，性能差
- Redis 锁是 `AP` 锁，可靠性差，性能好（[[Database/redis/redis分布式锁#redis 集群使用分布式锁|redis集群实现分布式锁时的一致性问题]]）

## 查看日志文件

```shell
java -cp /opt/zookeeper/lib/zookeeper-3.4.6.jar:/opt/zookeeper/lib/* org.apache.zookeeper.server.LogFormatter ./data/logs/version-2/log.2a00dab3b2
```

## 参考资料

https://segmentfault.com/a/1190000022861935
