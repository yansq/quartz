# 分布式 session

## 粘性会话(Sticky Session)

使用负载均衡机制，做 ip 保持。来自同一 ip 的请求只会落到同一台服务器上。

问题：
当一台机器宕机后，原本前往该机器的请求会被分配到集群的其他机器，而其他机器上并没有 session，导致报错。

## 会话复制（Session Replication）

当一台机器创建 session后，同步到其他机器。每台机器都拥有所有 session。

问题：
复制会话需要消耗资源，且复制存在时延，可能导致一致性问题。

## 集中会话（Centralized Session）

将会话集中到一处存储，常见的存储设施有：

- Redis
- MongoDB
- JDBC
- Hazelcast

