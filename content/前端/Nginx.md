# Nginx

## 命令行参数

- `-p`： 设置路径前缀，如 `/usr/local/nginx`
- `-q`：在配置测试期间禁止非错误信息

## if 指令参数

`=`：比较
`~`：匹配正则表达式
`-f` `!-f`：检查一个文件是否存在
`-d` `!-d`：检查一个目录是否存在
`-e` `!-e`：检查一个文件、目录、符号链接是否存在
`-x` `!-x`：检查一个文件是否可执行

## 进程模型

- master 进程：主进程
- worker 进程：工作进程，可以通过配置文件中的 worker_processes 来配置 worker 的进程数量

![image-20210404173625175](https://zq99299.github.io/note-architect/assets/img/image-20210404173625175.40037b3e.png)

为保证只有一个进程处理单个连接，**所有 worker 进程在注册 listenfd 读事件前抢 accept_mutex**，抢到互斥锁的 worker 进程注册 listenfd 读事件；在读事件里调用 accept 接受该连接。
nginx 使用异步非阻塞，只需要少量的 work 就可以处理大量的请求