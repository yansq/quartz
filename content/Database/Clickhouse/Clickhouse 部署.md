# Clickhouse 部署

## 部署示意图

![[Pasted image 20230918164928.png]]

## Clickhouse 配置

CPU：16C

内存：至少32GB，对于少量数据（最多约 200 GB 压缩数据），最好使用与数据量一样多的内存。对于大量数据以及处理交互式（在线）查询时，您应该使用合理数量的 RAM（128 GB 或更多），以便热数据子集适合页面缓存。即使每台服务器的数据量约为 50 TB，与 64 GB 相比，使用 128 GB RAM 也能显着提高查询性能。

节点数：除去最大规模的应用，两台足够，推荐纵向扩展

## 集群协调中间件

可选 ClickHouse Keeper 或 zookeeper ，推荐 Clikhouse Keeper 或 高版本 zookeeper

推荐生产环境单独部署，2C 4GB 即可