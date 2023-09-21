# mget

ES的批量get操作，减少网络开销，提高性能。

```json
GET _mget
{
    "docs": [
        {
            "_index": "user",
            "_id": 1
        }, 
        {
            "_index": "comment",
            "_id": 1
        }
    ]
}
```