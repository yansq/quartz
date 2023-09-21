# Pin & Unpin

在同步情况下，Rust 的借用机制足以保证内存引用的安全（只允许一个可变引用，可变引用与不可变引用不能存在于同一作用域中）。

在异步编程中，存在数据竞争问题，当多个任务访问和修改同一数据时，Rust 的借用规则无法静态检查异步任务之间的调度和执行顺序，因此无法提供对于数据竞争的静态保证。

## Pin

防止一个类型在内存中移动。`Pin` 是一个**结构体**：

```rust
pub struct Pin<P> {
    pointer: P,
}
```

`Pin` 包裹一个指针，并且能确保该指针指向的数据不会移动，例如 `Pin<&mut T>`，`Pin<&T>`，`Pin<Box<T>>`，都能确保 `T` 不会被移动。

### !Unpin

可以被 `Pin` 住的值实现的特征是 `!Unpin`。使用标记类型 `PhantomPinned` 可以自动让编译器为结构体实现 `!Unpin` 特征：

```rust
struct Test {
    a: String,
    // 裸指针
    b: *const String,
    _marker: PhantomPinned,
}
```

> [!note]  Pin 这个结构体并不是让 Pin 中的值变的不可移动, 而是让你无法在不使用 `unsafe` 方法的前提下改动 Pin 中的地址。 真正标志一个值是不是不可移动的只有 `!Unpin` 这个标志. 这就是 Pin 只是一个结构体, 而 `!Unpin` 是一个编译器自动实现的 trait 的原因。

## Unpin

`Unpin` 是一个特征，表明一个类型可以被随意移动，绝大多数类型都自动实现了 `Unpin` 特征。

实现 `Unpin` 特征的类型被 `Pin` 包裹后没有任何效果。