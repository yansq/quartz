## 启动命令

`nohup ./kibana &`

## 查找进程

无法使用`ps -ef | grep kibana`命令，因为kibana是用Node写的，进程名为node

使用`netstat -tunlp | grep 5601`查找进程