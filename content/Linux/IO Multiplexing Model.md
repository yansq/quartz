# I/O Multiplexing Model(多路复用)

![[Linux/attachments/CleanShot 2023-05-05 at 10.54.12@2x.png]]

-   Disadvantage: using `select` requires two system calls (`select` and `recvfrom`) instead of one
-   Advantage: we can wait for more than one descriptor to be ready 

## select

将已连接的 Socket 都放到一个[[Linux/File Descriptor|文件描述符]]集合，然后调用 select 函数将文件描述符集合拷贝到内核里，让内核来检查是否有网络事件产生，检查的方式很粗暴，就是通过遍历文件描述符集合的方式，当检查到有事件产生后，将此 Socket 标记为可读或可写， 接着再把**整个文件描述符集合**拷贝回用户态里，然后用户态还需要再通过遍历的方法找到可读或可写的 Socket，然后再对其处理。

select 使用固定长度的 BitsMap，表示文件描述符集合，而且所支持的文件描述符的个数是有限制的，在 Linux 系统中，由内核中的 FD_SETSIZE 限制， 默认最大值为 `1024`，只能监听 0~1023 的文件描述符。

## poll

和 select 差别不大，再用 BitsMap 来存储所关注的文件描述符，取而代之用动态数组，以链表形式来组织，突破了 select 的文件描述符个数限制，当然还会受到系统文件描述符限制。

## epoll

epoll 在内核里使用[[数据结构&算法/红黑树|红黑树]]来跟踪进程所有待检测的文件描述字，在更新时不需要全量拷贝，提升了性能。

内核里**维护了一个链表来记录就绪事件**，当某个 socket 有事件发生时，通过**回调函数**内核会将其加入到这个就绪事件列表中，当用户调用 `epoll_wait()` 函数时，只会返回有事件发生的文件描述符和个数，不需要像 select/poll 那样轮询扫描整个 socket 集合，大大提高了检测的效率。