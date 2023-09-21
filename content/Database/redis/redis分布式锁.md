# redis 分布式锁

#TODO 

https://juejin.cn/post/6844903830442737671#heading-15

https://juejin.cn/post/6936956908007850014#heading-7

## 使用 LUA 脚本

```shell
if redis.call('setnx',KEYS[1],ARGV[1]) == 1 then redis.call('expire',KEYS[1],ARGV[2]) else return 0 end;
```

## 使用 set 命令

```shell
SET key value[EX seconds][PX milliseconds][NX|XX]
```

不使用 `setnx` + `expire`的原因是：两步操作不是原子操作，如果在设置过期时间之前，应用异常，该锁永生。

使用该方式设置的`value`，须具有全局唯一性，可以使用`UUID`。如果 `value`不唯一，可能会引发操作的场景：

1. 客户端 a 获取锁成功
2. 客户端 a 在某个操作上阻塞了太长时间
3. 设置的 key 过期，锁自动释放
4. 客户端 b 获取到了同一个锁
5. 客户端 a 从阻塞中恢复，因为value值一样，所以释放锁时也会释放掉客户端 b 持有的锁，引发问题

## 使用 Redisson

![[Database/redis/attachments/Pasted image 20220707212943.png]]

只要线程一加锁成功，就会启动一个`watch dog`看门狗，它是一个后台线程，会每隔10秒检查一下，如果线程1还持有锁，那么就会不断的延长锁key的生存时间。因此，Redisson就是使用Redisson解决了**锁过期释放，业务没执行完**问题。

## redis 集群使用分布式锁

redis 集群使用上述方式设置分布式锁时，可能会产生问题：主节点 A 设置在分布式锁 a。当锁 a 还没被同步到对应的 slave 节点时，节点 A 故障，对应的 slave 节点升级为主节点，这时锁 a 仍可以被其他线珵获取。

在集群模式下，可以使用 [[redlock]] + `Redisson` 实现分布式锁。

redlock 原理，假设 redis 集群有 5 个 master 节点：

1. 获取当前时间，以毫秒为单位。
2. 按顺序向5个 master 节点请求加锁。客户端设置网络连接和响应超时时间，并且超时时间要小于锁的失效时间。（假设锁自动失效时间为10秒，则超时时间一般在5 - 50毫秒之间，假设超时时间为50ms）。如果超时，跳过该 master 节点，尽快去尝试下一个 master 节点。
3. 客户端使用当前时间减去开始获取锁时间（即步骤1记录的时间），得到获取锁使用的时间。当且仅当超过一半（N/2+1，这里是5/2+1=3个节点）的 Redis master 节点都获得锁，并且使用的时间小于锁失效时间时，锁才算获取成功。
4. 如果取到了锁，key 的真正有效时间就变啦，需要减去获取锁所使用的时间。
5. 如果获取锁失败（没有在至少N/2+1个master实例取到锁，或者获取锁时间已经超过了有效时间），客户端要在所有的 master 节点上解锁（即便有些master节点根本就没有加锁成功，也需要解锁，以防漏网之鱼）。
