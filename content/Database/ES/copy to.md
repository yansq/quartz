# copy to

The `copy_to` parameter allows you to copy the values of multiple fields into a group field, which can be queried as a single field. You can use copy_to to improve search speeds if you often search multiple fields.

```json
PUT my-index-000001
{
  "mappings": {
    "properties": {
      "first_name": {
        "type": "text",
        "copy_to": "full_name"
      },
      "last_name": {
        "type": "text",
        "copy_to": "full_name"
      },
      "full_name": {
        "type": "text"
      }
    }
  }
}
```

```json
GET my-index-000001/_search
{
  "query": {
    "match": {
      "full_name": { 
        "query": "John Smith",
        "operator": "and"
      }
    }
  }
}
```


https://www.elastic.co/guide/en/elasticsearch/reference/7.17/copy-to.html

https://cloud.tencent.com/developer/article/1697284
