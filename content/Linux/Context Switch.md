# Context switch

A **context switch** is the process of storing the state of a process or thread, so that it can be restored and resume execution at a later point, and then restoring a different, previously saved, state. This allows multiple processes to share a single CPU, and is an essential feature of a multitasking operating system.

Steps:

1.  保存处理机上下文，包括程序计数器和其他寄存器。
2.  更新PCB信息。
3.  把进程的PCB移入相应的队列，如就绪、在某事件阻塞等队列。
4.  选择另一个进程执行，并更新其PCB。
5.  更新内存管理的数据结构。
6.  恢复处理机上下文。
