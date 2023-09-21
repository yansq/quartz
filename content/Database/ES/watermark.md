# watermark

ES使用watermark来标记磁盘使用情况，配置位于`cluster.routing.allocation.disk.watermark`。

watermark共有三个阶段：`low`，`high`以及 `flood_stage`

- `low`：默认**85%**，超过low水位后，es不会将分配[[Database/ES/分片]]给该节点
- `high`：默认为**90%**，超过high水位后，es尝试将节点上的现有[[Database/ES/分片]]重新定位
- `flood_stage`：默认为**95%**，进行洪水泛滥阶段后，es对节点上[[Database/ES/分片]]对应的索引进行强制只读处理。 ^3fb866