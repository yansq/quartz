# 集群信息查看

ES中的api采用[[RESTful]]风格

`?v`：在返回结果中带上表头信息

GET  `/_cat/health?v`：查看集群状态

GET `/_cluster/allocation/explain`：查看分片未分配原因

GET  `/_cat/nodes?v`：查看节点列表 

GET  `/_cat/indices?v`：查看所有索引

PUT  `/customer`：创建一个名为“customer” 的索引

`curl -X PUT "localhost:9200/customer/_doc/1?pretty" -H 'Content-Type: application/json' -d '{  "name": "John Doe"}'` ：创建一个文档（指定ID）

`curl -X POST "localhost:9200/customer/_doc?pretty" -H 'Content-Type: application/json' -d'{  "name": "Jane Doe"}'`：创建一个文档（不指定ID）

ES查看内存占用：
```zsh
Get /_cat/nodes?h=name,heapCurrent,fielddataMemory,queryCacheMemory,requestCacheMemory,segmentsMemory&v
```

重新尝试分配分片：
```zsh
Get /_cluster/reroute?retry_failed
```

## Cat 结果在 Elasticseach-head 插件不显示

通过浏览器开发者工具，在网络请求返回中可以看到数据。
