不同的 Container 之间实现了进程隔离、网络隔离等，但共享同相同的 OS kernel。这也就意味着 Linux kernel 的 Docker Container 不能直接在 Windows 的机器上运行。事实上，它是运行在 Windows 系统的 Linux 虚拟机上。

Container 运行的必要条件是其中至少存在一个进程。如果一个 Container 中的所有进程都因故销毁，那么这个 Container 也就不会再继续处于运行状态。此时，只能使用 `docker ps -a` 命令，才能查找到这个 Container。

使用 `docker stop`命令来停止一个正在运行的 Container，该 Container 的相关数据会保存在硬盘中以供再次启动。如果要彻底清除一个 Container，使用 `docker rm` 命令。