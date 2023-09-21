# Mac 使用 redis

使用 [[homebrew]] 安装 redis：
```shell
brew install redis
```

安装后使用 `brew list redis` 查看安装位置

配置文件位于 `/opt/homebrew/etc` 目录

启动命令：
```shell
brew services start redis
```
或者：
```shell
redis-server /opt/homebrew/etc/redis.conf
```

连接 redis：
```shell
redis-cli -h 127.0.0.1 -p 6379
```

关闭 redis:
```shell
redis-cli shutdown
```