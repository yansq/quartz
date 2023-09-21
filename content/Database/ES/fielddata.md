# fielddata

fielddata 用于给 text 类型的字段做排序、聚合使用。它是通过读取磁盘上的每个 [[Database/ES/段（segment）]]的**整个**[[Database/ES/倒排索引]]来构建的，将 **term**（词条）和 [[Database/ES/文档（document）]]关系反转，并将结果存储在 **JVM** 堆中。 

加载 fielddata 需要占用大量的堆内存，以及会造成查询延时，默认是*关闭*的。

通常情况下，如果确实要对 text 字段来做排序和聚合，且这个字段已分词，导致无法进行这些操作，应该创建一个 [[Database/ES/keyword]] 子字段来用做排序和聚合，而不是开启`fielddata`。

