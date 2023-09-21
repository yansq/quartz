
使用String形式进行ES搜索

```json
GET /gcrm_acct_prod/_search
{
    "stored_fields": ["*"],
    "query": {
        "query_string": {
            "default_field": "ACCT_ID",
            "query": "11 OR 22"
        }
    }
}
```

另有Simple Query String 模式，在该模式下禁用`AND`、`OR`等查询语法。
