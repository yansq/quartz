# Synonym token filter（同义词）

## Configuration

```json
{
    "analysis": {
        "filter": {
            "synonym": {
                "type": "synonym",
                "lenient": true,
                "synonyms": [
                    "波行, 宁波银行",
                    "招行 => 招商银行"
                ]
            }
        },
        "analyzer": {
            "text_char": {
                "tokenizer": "char_tokenizer",
                filter: [
                    "lowercase",
                    "whitespace_remove",
                    "synonym"
                ]
            }
        },
        "tokenizer": {
            "char_tokenizer": {
                "type": "simple_pattern",
                "pattern": "."
            }
        }
    }
}
```

- `"招行 => 招商银行"`：single direction
- `"波行，宁波银行"`：double direction

Configuration with txt file：

```json
"filter": {
    "synonym": {
        "type": "synonym",
        "synonyms_path": "analysis/synonym.txt"
    }
}
```

## Analyzer workflow

Character filters => Tokenizer => Token filters

> [!warning] Token filters are not allowed to change the position or character offsets of each token.

```json
_analyze {
    "anayzer": "text_char",
    "text": ["波行"]
}
```

Result: 

```json
{
    "tokens": [
        {
            "token": ”波“，
            ”start_offset“: 0,
            "end_offset": 1,
            "type": "word",
            "position": 0
        },
         {
            "token": ”宁“，
            ”start_offset“: 0,
            "end_offset": 1,
            "type": "SYNONYM",
            "position": 0
        },
         {
            "token": ”行“，
            ”start_offset“: 1,
            "end_offset": 2,
            "type": "word",
            "position": 1
        },
         {
            "token": ”波“，
            ”start_offset“: 1,
            "end_offset": 2,
            "type": "SYNONYM",
            "position": 1
        },
         {
            "token": ”银“，
            ”start_offset“: 1,
            "end_offset": 2,
            "type": "SYNONYM",
            "position": 2
        },
         {
            "token": ”行“，
            ”start_offset“: 1,
            "end_offset": 2,
            "type": "SYNONYM",
            "position": 3
        },
    ]
}
```

Will get in query profile:

```json
"description": "CUST_NM.char: \"(波 宁) (行 波) 银 行\""
```

## Reference

[Synonym token filter](https://www.elastic.co/guide/en/elasticsearch/reference/current/analysis-synonym-tokenfilter.html)
[借助同义词让 Elasticsearch 更加强大 | Elastic Blog](https://www.elastic.co/cn/blog/boosting-the-power-of-elasticsearch-with-synonyms)