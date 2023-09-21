# DST

DST(dynamically sized types) 指动态类型。编译器无法在编译期得知该类型值的大小，只有到程序运行时，才能动态获知。

常见的 DST 类型：
- str
- \[T\] 切片
- dyn Trait

DST 类型因为无法确定大小，无法办法直接在栈上存储，必须要通过引用或者`[[Rust/Box|Box]]`来间接使用。

## 在泛型中使用动态数据类型

```rust
fn generic<T: ?Sized>(t: &T) {
    // --snip--
}
```

此时参数就必须是引用类型了。

