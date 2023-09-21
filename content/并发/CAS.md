
CAS 是 compare and swap 的意思，是 CPU 指令级别的一个原子操作。将旧值与目标内存地址中的值进行比较，如果一致，就说明没有其他线程修改过这个值，将新值更新到内存地址中；如果不一致，不会进行更新，而是返回当前的值，然后**再次执行** CAS 操作。

当多个线程同时尝试使用 CAS 更新同一个变量时，其中一个线程会成功更新变量的值，剩下的会失败。

CAS 的缺点有：

- 如果循环的次数过多，对性能开销大；
- CAS 只能保证一个共享变量的原子性；
- 会引发 [[ABA]] 问题。

在 Java 中的 atomic 类实现 了 CAS 方法：
```java
private AtomicBoolean locked = new AtomicBoolean(false);
public boolean lock() {
    return locked.compareAndSet(false, true);
}
```
`AtomicLong`、`AtomicReference` 等 atomic 类也有类似实现。