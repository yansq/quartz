# MySQL 数据格式

- INT：integer
- FLOAT：single-precision floating point number
- DOUBLE：double-precision floating point number
- DECIMAL(M, D)：fixed-point number
- CHAR(M)：fixed-length string
- VARCHAR(M)：variable-length string，M for the longest length of string
- TEXT：小型的字符串
- ENUM()：枚举，只能选取列表中的其中一个
- SET()，可以选择列表中的多个字段
- BIT(M)：存储M个二进制
- BLOB：存储字节

有些小数，如 0.3，无法转换为二进制。所以在用浮点数存储时，存在精度丢失。
定点数将十进制小数用小数点隔开，分别存储小数点左右的两个十进制整数。

