# Option

Rust 中没有 `null` 值，取而代之的是 `Option`，这是一个[[Rust/枚举|枚举]]变量。

```rust
enum Option<T> {
    Some(T),
    None,
}
```

使用：
```rust
let some_number = Some(5);
let some_string = Some("a string");

// 赋值为 None 时需要指定类型
let absent_number: Option<i32> = None;

// 获取 Option 中的值
match some_number {
    Some(value) => println!("{}", value),
    None => (),
}

// 或是
if let Some(value) = some_number {
    println!("{}", value)
}
```

 `Option` 与 [[Rust/Result|Result]] 之间可以通过 `.ok()`、`.ok_or(())`相互转换。


