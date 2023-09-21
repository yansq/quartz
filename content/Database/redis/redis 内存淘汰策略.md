# redis 内存淘汰策略

当内存不足以容纳新写入数据：

- no-evicition（**默认策略**）：新写入操作会报错。
- allkeys-lru：移除最近最少使用的 key。
- allkeys-random：随机移除某个 key。
- volatile-lru：在设置了过期时间的键空间中，移除最近最少使用的 key。
- volatile-random：在设置了过期时间的键空间中，随机移除某个 key。
- volatile-ttl：在设置了过期时间的键空间中，有更早过期时间的 key 优先移除。

获取当前内存淘汰策略：
```shell
config get maxmemory-policy
```

通过命令修改淘汰策略：
```shell
config set maxmemory-policy allkeys-lru
```