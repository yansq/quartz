# profile API

通过profile API，可以看到ES的一个搜索情况，是如何拆分成底层的[[Lucene]]请求的，并且会显示每部分的耗时情况，可以用于查询优化以及问题排查。

在`query`上方设置`profile: true`，来开启profile API：
```json
GET /myIndex/_search
{
    "profile": "true",
    "query": {
        "match": {
            "author": "Tom"
        }
    }
}
```

