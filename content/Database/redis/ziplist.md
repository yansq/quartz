# ziplist (压缩列表)

![[Pasted image 20220721091529.png]]

- `zlbytes`字段的类型是 uint32_t, 这个字段中存储的是整个 ziplist 所占用的内存的字节数
- `zltail`字段的类型是 uint32_t, 它指的是 ziplist 中最后一个 entry 的偏移量. 用于快速定位最后一个 entry, 以快速完成 pop 等操作
- `zllen`字段的类型是 uint16_t, 它指的是整个 ziplit 中 entry 的数量. 这个值只占2bytes（16位）: 如果 ziplist 中 entry 的数目小于65535(2的16次方), 那么该字段中存储的就是实际 entry 的值. 若等于或超过65535, 那么该字段的值固定为65535, 但实际数量需要去遍历所有 entry 才能得到.
- `zlend`是一个终止字节, 其值为全 F, 即`0xff`。ziplist 保证任何情况下, 一个 entry的首字节都不会是 255

## entry 数据结构

- 一般格式：`<prevlen> <encoding> <entry-data>`
- 存储 int 类型：`<prevlen> <encoding>`

- `prevlen`：前一个 entry 的大小；
- `encoding`：不同的情况下值不同，用于表示当前 entry 的类型和长度；
- `entry-data`：真是用于存储 entry 表示的数据；

## 为什么 ZipList 省内存

ziplist 的长度可变，传统 list 的每个元素固定长度

## 缺点

- ziplist 不预留内存空间, 并且在移除结点后,立即缩容, 意味着每次写操作都会再分配内存
