# redis 为什么快

- 数据放在内存
- C 语言实现
- 单线程，避免线程切换的开销（CPU切换，锁竞争）
- [[Linux/IO Multiplexing Model|IO 多路复用]]
- 数据结构高效，底层有多种实现