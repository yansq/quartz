# ES 生产配置

## JVM

- 64位JVM
- Xms 与 Xmx 设置成一样，避免 heap resize 时引发停顿
- Xmx 设置不要超过物理内存的50%，单个节点最大内存不要超过32GB
- 使用 Server 模式
- 关闭 JVM Swapping

