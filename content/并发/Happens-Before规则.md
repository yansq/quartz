# Happens-Before 规则

Happens-Before 规则约束了编译器的优化行为，规定某个线程修改的变量何时对其他线程可见。将 Happens-Before 翻译为中文，大致为“**前面一个操作的结果对后续操作是可见的**”。

## 1 程序的顺序性原则

在一个线程中，按照顺序，前面的操作 Happens-Before 于后续的任意操作。

## 2 volatile 变量规则

对一个 [[volatile]] 变量的写操作，Happens-Before 于后续对这个 [[volatile]] 变量的读操作。

## 3 传递性

如果 A Happens-Before B，且 B Happens-Before C，那么 A Happens-Before C。

## 4 管程中锁的规则

对一个锁的解锁 Happens-Before 于后续对这个锁的加锁。
管程是一种通用的同步原语，在 Java 中， [[并发/synchronized]] 是对管程的实现。

## 5 线程 start() 规则

主线程 A 启动子线程 B 后，子线程 B 能够看到主线程在启动子线程 B 前的操作。

## 6 线程 join() 规则

主线程 A 等待子线程 B 完成（主线程 A 通过调用子线程 B 的[[thread.join]]方法实现），当子线程 B 完成后（主线程 A 中 join() 方法返回），主线程能够看到子线程对于共享变量的操作。
