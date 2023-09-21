# InnoDB 行

InnoDB 共有4种不同的行格式：COMPACT、REDUNDANT、DYNAMIC 和 COMPRESSED。

## COMPACT

COMPACT 行共分为4个部分：

- 变长字段长度列表：**逆序**存放所有变长字段的长度信息
- NULL 值列表：每个允许存储 null 的列对应一个二进制位，**逆序**存放
- 记录头信息
    - deleted_flag：标记记录是否被删除
    - min_rec_flag：标记 [[B+ Tree]] 每层非叶子节点中最小目录项的记录
    - n_owned：[[Database/InnoDB 页]]中每个组的头部记录
    - heap_no：当前记录在页面堆中的相对位置
    - record_type：记录类型，0 普通记录，1 [[B+ Tree]]非叶子节点， 2 Infimum 记录， 3 Supremum 记录
    - next_record：下一条记录的相对位置
- 真实数据


