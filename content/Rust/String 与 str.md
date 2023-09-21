# String 与 str

String 的本质是一个 u8 的[[Rust/Vector|Vector]]。

```rust
struct String {
    vec: Vec<u8>,
}
```

Q：为什么是 u8？
A：String 以 utf-8 字符存储，这样可以节省内存。并不是所有 utf-8 字符都会占用到 char 的4个字节的存储空间。

Vec 拥有内存的所有权，因此可以对 String 进行修改。当 String 超出作用域后，内存即被回收。

```rust
// String
let s = String::from("hello world");
// &str
let world = &s[6..11];
```

以上代码的内存分布：

![[Rust/attachments/Pasted image 20230809101333.png|400]]

图的右边部分即为 `str`。

Rust 在语言级别，只有一种字符串类型： `str`。`str` 是一个动态、不可变变量。如果 str 由 String 转换过来，存储在堆上，如果有静态字面量创建，存储在静态内存。
- 动态：分配给 str 的内存不确定，"aaa"：3个字节；“bbbbb“：5个字节
- 不可变：创建后无法修改

由于 str 大小是动态的，无法直接使用，通常使用的是 &str。&str 包括一个指针，和一个长度字段，其大小在编译期已知。 &str 对指向的 str 没有所有权，属于借用（borrow）关系，因此只读，无法修改。切片被丢弃后，不会执行任何操作。

Rust 编辑器会将 `&String` 解引用为 `&str`。 

在通过函数传递参数时，或者在循环中，如果使用`String`，会导致内存的重新分配，造成性能损耗。
