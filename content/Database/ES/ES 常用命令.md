# ES 常用命令

解除只读限制：
```json
PUT /_all/_settings
{
    "index.blocks.read_only_allow_delete": null
}
```

动态修改副本数：
```json
PUT /INDEXNAME/_settings
{
    "number_of_replicas": 0
}
```


> 在 ES 因磁盘占满，进入[[Database/ES/watermark#^3fb866|flood_stage]]状态时，无法修改索引的副本数。这时需要先将只读设置解除，才能删除副本。


查看集群设置，包含默认设置：
```json
GET /_cluster/settings?include_defaults
```

