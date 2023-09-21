# 刷新（refresh）

ES使用刷新机制来实现**近实时**的全文搜索。

每当有数据更新时，ES先将数据存储在内存中，当达到默认的时间（1秒）、或内存中的数据达到一定量的时候，会触发刷新，将数据从ES的JVM内存（Index Buffer）移动到文件缓存系统（操作系统内存）上，形成一个[[Database/ES/段（segment）]]。

可以通过`POST /index/_refresh`手动触发刷新。
配置`refresh_interval='30s'`用于调节自动刷新时间，不带单位时默认单位为毫秒


