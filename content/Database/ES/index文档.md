
```json
PUT users/_doc/1
{
    "tags": ["guitar", "skateboard", "reading"]
}
```

## index与create的区别：

如果文档不存在，就创建新的文档。否则现有文档会被删除，新的文档被创建，版本信息（version）加1。

## index与update的区别：

index会删除原有文档的source，重新创建；update会在原来文档的基础上进行更新。假设原文档有字段a，index与update操作只填写字段b的信息，index操作下，文档的字段a被删除；而update操作下，字段a仍被保留，新增字段b。

- index 使用 put 操作
- update 使用 post 操作

当关闭 `source`后，不能进行`update`操作，只能进行`index`操作。