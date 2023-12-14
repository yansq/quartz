# keyword

ES字段类型，用于存储结构化内容，通过用于term查询以及排序，聚合操作。

`Keyword` fields are not [[analyzing data#normalization|normalized]], as the `normalizer` property on the `keyword` type is set to `null`. But we can customize and enable `normalizer` on the `keyword` data type by setting filtes such as `german_normalizer`, `uppercase` , and so on.

keyword字段不会对字段值进行默认分词。

每个text字段会默认添加一个keyword的字段。