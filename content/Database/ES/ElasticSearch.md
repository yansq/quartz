# ElasticSearch

基于Lucene的分布式搜索引擎，基于RESTful web接口，用JAVA开发。

## 概念

 * 集群
 * 节点
 * [[Database/ES/分片]]
 * 副本
 
![[Database/attachments/Pasted image 20210414083312.png]]

ES中的Documents对应传统数据库中的行，Fields对应传统数据库中的列。

## 特点

 ES查询远快于传统数据库的主要原因在于[[Database/ES/倒排索引]]。

写入时按什么分词，查询的时候就按什么分词。

## 应用场景

* 订单搜索
* 商品推荐
* **日志管理**
* **APM(应用系统性能监控)**
* 风险控制
* IT运维


