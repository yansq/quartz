# ES读写优化设置

写优化：

- 只需要聚合不需要搜索的字段，`index`设置成false
- 不需要算分的字段，`norms`设置成false
- 关闭字符串的[[Database/ES/dynamic mapping]]功能，减少字段数量
- 使用[[Database/ES/mapping设置#index_options|index_option]]控制哪些字段会被添加到倒排索引中，可以一定程度的节约CPU
- 关闭`_source`，减少IO操作，但会导致[[Database/ES/index文档]]与update操作失效，适合指标型数据
- 调大`refresh_interval`的数值，减少[[Database/ES/刷新（refresh）]]频率，牺牲实时性（需同时调整`indices.memory.index_buffer_size`）
- 单个[[Database/ES/bulk]]请求体的数据量不要太大，官方建议5-15mb
- 写入端的[[Database/ES/bulk]]请求超时要足够长，建议60s以上
- 写入端尽量将数据轮询打到不同节点

- 索引创建属于计算密集型任务，应该使用固定大小的线程池来配置，来不及处理的放入队列。
- 线程数应该配置成 CPU 核心数+1，避免过多的上下文切换
- 队列大小可以适当增加，不要过大，否则占用的内存会成为GC的负担

读性能优化：

- 尽量使用 Filter Context，利用缓存机制，减少不必要的算分
- 严禁使用`*`开头通配符的 term 查询

