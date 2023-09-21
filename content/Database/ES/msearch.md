# msearch

ES 的批量 search 操作

```json
POST my_index/_msearch
{}
{ "query": {"match_all": {}}, "size": 1 }
{ "index": "his_index" }
{ "query": {"match_all": {}}, "size": 2 }
```

建议批量查询的文档在1000-5000个文档，大小控制在5-15MB。