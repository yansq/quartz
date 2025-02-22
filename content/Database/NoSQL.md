# NoSQL

常见的NoSQL类型：

- 键值存储：数据存储在键值对数组中。包括：Redis，Voldmor，Dynamo
- 文档数据库：数据存储在文档中，每个文档可以具有完全不同的结构。包括：CouchDB，MongoDB，ES
- 宽列数据库：使用列族存储数据，列族是行的容器，每一行可以具有不同的列数。列式数据库适合分析大型数据集，包括：Cassandra，HBase
- 图数据库：数据保存在具有节点，属性和线的图形结构中，包括：Neo4J，InfiniteGraph

## 特性

NoSQL数据库是可水平扩展的，可以轻松在NoSQL架构中添加更多服务器来处理大流程，相比垂直扩展成本更低。

大多数NoSQL架构因为性能和可扩展性，牺牲了[[Database/ACID]]特性。

## 什么场景使用NoSQL

- 存储没有结构的大量数据
- 充分利用云计算和存储
- 快速发展，适用于数据结构频繁更新的场景
