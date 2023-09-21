## Create index

```json
PUT test
{
    "mappings": {
        "properties": {
            "my_join_field": {
                "type": "join",
                "relations": {
                    "my_parent": "my_child"
                }
            }
        }
    }
}
```

## Add data

```json
PUT test/_doc/1?refresh
{
    "pNum": "aaa",
    "my_join_field": "my_parent"
}

PUT test/_doc/2?routing=1&refresh
{
    "cNum": "bbb",
    "my_join_field": {
        "name": "my_child",
        "parent": 1
    }
}
```

## Join-Search

```json
POST test/_search
{
    "query": {
        "bool": {
            "must": [
                {
                    "has_child": {
                        "type": "my_child",
                        "query": {
                            "match": {
                                "cNum": "bbb"
                            }
                        },
                        "inner_hits": {}
                    }
                },
                {
                    "match": {
                        "pNum": "aaa"
                    }
                }
            ]
        }
    }
}
```

## Result

```json
"hits": [
    {
        "_source": {
            "pNum": "aaa",
            "my_join_field": "my_parent"
        },
        "inner_hits": {
            "my_child": {
                "hits": {
                    ...
                    "hits": [{
                        "_source": {
                            "cNum": "bbb",
                            "my_join_field": {
                                "name": "my_child",
                                "parent": 1
                            }
                        }
                    }]
                }
            }
        }
    }
]
```
