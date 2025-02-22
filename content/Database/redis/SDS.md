# SDS（简单动态字符串）

Redis 没有使用C语言的默认类型，而是自己实现了一个。

sds 包括3个部分：头部，数据部，以及末尾的一个`\0`。

sds 包括3个部分：头部，数据部，以及末尾的一个`\0`。

![sds 结构](https://tva1.sinaimg.cn/large/e6c9d24ely1h103i33ptrj20eo01nmwz.jpg)

## SDS 头部

- `len` 保存了 SDS 保存字符串的长度
- `alloc`分别以 uint8, uint16, uint32, uint64 表示整个 SDS, 除过头部与末尾的`\0`, 剩余的字节数
- `flags` 始终为1字节, 以低三位标示着头部的类型, 高5位未使用.

## 为什么使用 SDS

- *常数复杂度*获取字符串长度：由于 `len` 属性，获取 SDS 字符串的长度只需要读取 len 属性，时间复杂度为 `O(1)`。而对于 C 语言，获取字符串的长度是经过遍历计数实现的，时间复杂度为 `O(n)`。通过 `strlen key` 命令可以获取 key 的字符串长度。
- 杜绝缓冲区溢出： C 语言中使用 `strcat` 函数来进行两个字符串的拼接，一旦没有分配足够长度的内存空间，就会造成缓冲区溢出。而对于 SDS 数据类型，在进行字符修改的时候，**会根据记录的 len 属性检查内存空间是否满足需求**，如果不满足，会进行相应的空间扩展，然后在进行修改操作，所以不会出现缓冲区溢出。
- 减少修改字符串的内存重新分配次数：C 语言不记录字符串的长度，所以如果要修改字符串，必须重新分配内存（先释放再申请），因为如果没有重新分配，字符串长度增大时会造成内存缓冲区溢出，字符串长度减小时会造成内存泄露。SDS 实现了**空间预分配**和**惰性空间释放**两种策略。
    - 空间预分配：对字符串进行空间扩展的时候，扩展的内存比实际需要的多，这样可以减少连续执行字符串增长操作所需的内存重分配次数。
    - 惰性空间释放：对字符串进行缩短操作时，程序不立即使用内存重新分配来回收缩短后多余的字节，而是使用 `alloc` 属性将这些字节的数量记录下来，等待后续使用。
- 二进制安全：因为 C 字符串以空字符作为字符串结束的标识，而对于一些二进制文件（如图片等），内容可能包括空字符串，因此 C 字符串无法正确存取；而所有 SDS 的 API 都是以处理二进制的方式来处理 `buf` 里面的元素，并且 SDS 不是以空字符串来判断是否结束，而是以 len 属性表示的长度来判断字符串是否结束。

