# Index Statistics

command:

```shell
GET cars/_stats?human
```

results:

```json
{
    "_shards": {...},
    "_all": {
        "primaries": {
            "docs": {...},
            "store": {...},
            "indexing": {...},
            "get": {...},
            "search": {...},
            "merges": {...},
            "refersh": {...},
            "flush": {...},
            "warmer": {...},
            "query_cache": {...},
            "fielddata": {...},
            "completion": {...},
            "segments": {...},
            "translog": {...},
            "request_cache": {...},
            "recovery": {...}
        },
        "total": {...}
    },
    "indices": {
        ...
    }
}
```