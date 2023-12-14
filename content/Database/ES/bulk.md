# bulk

使用bulk API，可以对ES进行批量操作。

bulk支持4种操作类型：
- Index
- Create
- Update
- Delete

如果在bulk命令执行过程中，单条操作失败，不会影响其他的操作。
在bulk的返回结果中，包含每一条操作的执行结果。

```json
POST _bulk
{ "index": {"_index": "test", "_id": "1"} }
{ "field1": "value1" }
{ "delete": {"_index": "test", "_id": "2"} }
{ "create": {"_index": "test2", "_id": "3"} }
{ "field1": "value3" }
{ "update": {"_id": "1", "_index": "test"} }
{ "doc": {"field2": "value2"} }
```

`_bulk`中的一条操作指令可能占一行或两行。

