# ReentrantLock

#多线程 

`java.util.concurrent`包中的内容，用于代替[[Java/synchronized]]，也是可重入锁。

```java
public class Counter {
    private final Lock lock = new ReentrantLock();
    private int count;

    public void add(int n) {
        lock.lock();
        try {
            count += n;
        } finally {
            lock.unlock();
        }
    }
}
```

`ReentrantLock`新增了`trylock(time, TimeUnit.SECONDS)`方法，如果在`time`时间内没有获取到锁，就会返回`false`。 ^47c16d

```java
if (lock.tryLock(1, TimeUnit.SECONDS)) {
    try {
        ...
    } finally {
        lock.unlock();
    }
}
```