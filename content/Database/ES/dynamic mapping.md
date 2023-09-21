# dynamic mapping(动态映射)

当新增字段，又没有指定字段类型时，ES会自动推断新字段的类型。

使用`mapping.dynamic`配置项配置ES的动态映射行为：
- true：新增文档可被索引，新增字段可被索引，mapping配置被更新
- false：新增文档可被索引，新增字段进入source但无法被搜索，mapping配置不更新
- strict：数据写入报错

