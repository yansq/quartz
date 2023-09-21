# Rc\<T\>

引用计数（reference counting）智能指针，记录数据被引用的次数。当引用次数归零时，代表数据不再被使用，可以被清理释放。

错误例子：
```rust
fn main() {
    let s = String::from("hello world");
    let a = Box::new(s);
    // 报错！所有权已被转移给 a
    let b = Box::new(s);
}
```

使用`RC<T>`：
```rust
use std::rc::Rc;

fn main() {
    let a = Rc::new(String::from("hello world"));
    let b = Rc::clone(&a);
    assert_eq!(2, Rc::strong_count(&a));
    assert_eq!(Rc::strong_count(&a), Rc::strong_count(&b));
    println!("{}", a);
    println!("{}", b);
}
```

由于`Rc<T>`智能指针，实现了 [[Rust/Deref|Deref]] 特征，因此可以直接使用。

> [!note] `Rc::clone()`仅复制了智能指针并增加计数，没有克隆底层数据。

`Rc<T>` 是指向底层数据的不可变的引用，因此无法通过它来修改数据；如果想要修改数据，需配合内部可变性`RefCell`或互斥锁`Mutex`。

