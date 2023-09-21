# AQS

AQS 指的是 `AbstractQueuedSynchronizer` ，队列同步器，这是个抽象类。它定义了同步状态，以及获取和释放的方法。常用的 [[ReentrantLock]] / `Semaphore` / `CountDownLatch` 都依赖了这个抽象类（内部类继承 AQS，不是直接继承）。

AQS 维护了一个 `volatile int state`，代表竞争变量标识；以及一个 FIFO 的双向线程等待队列。

```java
public abstract class AbstractQueuedSynchronizer extends AbstractOwnableSynchronizer implements java.io.Serializable {
	...
	/**
	 * The synchronization state.
	 */
	private volatile int state;
	/**
     * Wait queue node class.
     **/
     static final class Node {
		...
	}
	...
}
```

AQS 有两种资源共享模式：
1. *独占式*，每次只能有一个线程持有锁，例如 [[ReentrantLock]] 就是独占式的；
2. *共享式*，允许多个线程同时获取锁，并发访问共享资源，`ReentrantReadWriteLock`和 `CountDownLatch`  等实现的就是这种方式。

 如果 AQS 为*独占式*，`state=0`表示资源未被锁定，当其他线程来竞争锁的时候，该线程会被加入到队列中。当执行线程执行`ublock`方式将 `state`设置为0后，其他线程才有机会获取锁。

如果是*共享式*，`state`的初始值与子线程的数量一致。每个子线程执行完以后，`state`会通过 [[CAS]] 的方式减1。直到`state=0`，会通过`unpark()`方式唤醒主线程，然后主线程就会从`await()`的方式返回，继续后续操作。