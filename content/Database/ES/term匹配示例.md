# term匹配示例

## 分词

mapping:
```json
PUT my_index { 
    "mappings": 
        { "my_type": 
         { "properties": 
          { "full_text": { "type": "string" }, 
            "exact_value": { "type": "string", "index": "not_analyzed" } 
          } 
         } 
        } 
} 

PUT my_index/my_type/1 { "full_text": "Quick Foxes!", "exact_value": "Quick Foxes!" }
```

默认字段，即`full_text`字段是被分析过的，`Quick Foxes!`被分词为`[quick, foxes]`；加了`"not_analyzed"`的`exact_value`是没有被分析过的。

使用如下查询语句：
```json
GET my_index/my_type/_search { "query": { "term": { "exact_value": "Quick Foxes!" } } }
```
使用`term`匹配`exact_value`字段时，由于没有分词，可以正确匹配。

但如果：
```json
GET my_index/my_type/_search { "query": { "term": { "full_text": "Quick Foxes!" } } }
```
匹配`full_text`字段时，因为已被分词，所以无法匹配到结果。

## 大小写

ES会对text内容默认做分词处理，其中会将大写字母转换为小写。在用term进行匹配的过程中，如果使用大写字母，将无法匹配到正确结果：
```json
POST /products/_search
{
    "query": {
        "term": {
            "phoneType": {
                "value": "iPhone"
             // "value": "iphone" (这样可以匹配到)
            }
        }
    }
}
```

可以使用[[Database/ES/keyword]]查询来避免这一问题：
```json
POST /products/_search
{
    "query": {
        "term": {
            "phoneType.keyword": {
                "value": "iPhone"
             // "value": "iphone" (这样可以匹配到)
            }
        }
    }
}
```

## 打分

使用term查询默认会返回算分结果，如果不需要算分结果，可以将Query转换为Filter，减少计算开销：

```json
POST /products/_search
{
    "query": {
        "constant_score": {
            "filter": {
                "term": {
                    "phoneType.keyword": {
                        "value": "iphone"
                    }
                }
            }
        }
    }
}
```