# Java线程状态

#多线程

- New：新创建的线程，尚未执行
- Runnable：运行中的线程，正在执行`run()`方法的Java代码
- Blocked：运行中的线程，因为某些操作被阻塞而挂起
- Waiting：运行中的线程，因为某些操作在等待中 ^070519
- Timed Waiting：运行中的线程，因为执行  `sleep()`方法正在计时等待
- Terminated：线程已终止，因为 `run()`方法执行完毕

线程Blocked和Waiting的区别？

- Blocked：一个线程即将进入[[synchronized]]块，但有另一个线程在同一个[[synchronized]]块中执行，第一个线程必须等待另一个线程退出。
- Waiting：一个线程正在等待另一个线程的信号，线程通常使用[[wait()&notify()&notifyAll()#wait|wait()]]或者[[thread.join]]方法进入该状态，当另一个线程调用[[wait()&notify()&notifyAll()#notify|notify()]]方法或死亡后，该线程会被唤醒。


## 方法&关键字

`thread.start()`

`thread.interrupt()`

## 乐观锁和悲观锁

## 什么是CAS（Compare and Set）

