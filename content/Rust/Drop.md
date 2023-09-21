# Drop

智能指针实现了`Drop`指针，用于释放资源。

Rust 几乎为所有类型都实现了`Drop`特征，当变量超出作用域时，会自动调用`drop(&mut self)`方法。

## 手动回收资源

```rust
#[derive(Debug)]
struct Foo;

impl Drop for Foo {
    fn drop(&mut self) {
        println!("Dropping Foo!")
    }
}

fn main() {
    let foo = Foo;
    // 报错！
    foo.drop();
    println!("Running!:{:?}", foo);
}
```

方法`foo.drop()`只是借用了目标值的可变引用，所以在方法之后再使用目标值，会引发内存安全问题。Rust 编译器拒绝这种做法，而推荐使用`drop(foo)`函数。

`drop(foo)`函数的签名为`fn drop<T>(_x: T)`，获取了目标的所有权，更加安全。该方法不释放内存，只是抢夺所有权。实际释放资源仍然是在离开作用域时，调用`std::ops::Drop::drop()`实现。

## Drop 与 Copy 互斥

无法为一个类型同时实现 `Copy` 和 `Drop` 特征。实现了 `Copy` 的特征会被编译器隐式复制，因此非常难以预测[[Rust/析构函数|析构函数]]执行的时间和频率。因此这些实现了 `Copy` 的类型无法拥有析构函数。