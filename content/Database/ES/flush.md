# flush

ES的 flush 操作，即 Lucene 的 commit操作，将[[Database/ES/段（segment）]]从文件缓存中写入磁盘。

- 执行flush操作时会默认调用一次[[Database/ES/刷新（refresh）]]操作
- 调用 fsync，将[[Database/ES/段（segment）]]写入磁盘
- 清空[[Database/ES/Transaction Log]]

flush默认每30分钟调用一次。
当[[Database/ES/Transaction Log]]满（默认512MB）时，也会触发。