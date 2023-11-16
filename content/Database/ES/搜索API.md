# 搜索API

## 两种查询方式

ES有两种方法执行搜索：
1. 通过URI传递搜索参数
2. 通过请求体传递搜索参数

使用URI方式：
`curl -X GET "localhost:9200/bank/_search?q=*&sort=account_number:asc&pretty"`

使用请求体方式：
`curl -X GET "localhost:9200/bank/_search" -H 'Content-Type: application/json' -d'{  "query": { "match_all": {} },  "sort": [    { "account_number": "asc" }  ]}'`

## 查询关键字

- `_source`：返回文档中的哪些字段，如`"_source": ["account_number", "balance"]`
- `match`: 字段是否匹配（包含），如`"query":{ "match": {address: "mill lane"} }`（address字段中是否包含mill或lane），ES会进行对查询进行分词，然后匹配，任一分词结果命中都会返回
- `match_phrase`：同`match`，所有分词结果命中，并保证顺序正确，可通过`slop`调节匹配强度
- `term`：代表完全匹配，不进行分词。使用`term`查询需要确认字段是否被分析（analyzed），详见[[Database/ES/term匹配示例|示例]]。
- `bool`：执行布尔逻辑

match_phrae:
```json
"query": {
  "bool": {
    "should": [{
        "match_phrase": {
           "CUST_NM.char: {
             "query": \"波行\"",
             "slop": 1
        }
    }]
  }
}
```

搭配使用`must`进行与运算，另有`must_not`子句：
```json
curl -X GET "localhost:9200/bank/_search" -H 'Content-Type: application/json' -d'{  "query": {    
    "bool": {      
        "must": [        
            { "match": { "address": "mill" } },
            { "match": { "address": "lane" } }      
        ]    
    }  
}}'
```

搭配使用`should`进行或运算：
```json
curl -X GET "localhost:9200/bank/_search" -H 'Content-Type: application/json' -d'{  "query": {    
    "bool": {      
        "should": [        
            { "match": { "address": "mill" } },
            { "match": { "address": "lane" } }      
        ]    
    }  
}}'
```

- `filter`，使用效果与`must`类似，但在执行时不会像`must`那样对记录评分，执行效率会高很多
- `aggs`：执行聚合函数

按`state`分组，返回降序排列的前10个状态（默认）；设置`"size": 0`，为了只返回分组结果，不返回具体数据：
```json
curl -X GET "localhost:9200/bank/_search" -H 'Content-Type: application/json' -d'{  "size": 0,  "aggs": {    "group_by_state": {      "terms": {        "field": "state.keyword"      }    }  }}'
```

在上一条查询的基础上，对余额求平均值，并指定降序排列
```json
curl -X GET "localhost:9200/bank/_search" -H 'Content-Type: application/json' -d'{  
        "size": 0,  
        "aggs": {    
            "group_by_state": {      
                "terms": {        
                    "field": "state.keyword",        
                    "order": {          
                        "average_balance": "desc"        
                    }      
                },      
                "aggs": {        
                    "average_balance": {          
                        "avg": {            
                            "field": "balance"          
                        }        
                    }      
                }    
            }  
        }}'
```

