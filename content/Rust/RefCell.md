# RefCell

相较于[[Rust/Cell|Cell]]，并不需要数据实现 Copy 特征，而用于引用。

`RefCell` 只是将借用规则从编译期推迟到程序运行期，并不能绕过这个规则。

`RefCell` 适用于编译期误报或者一个引用被在多处代码使用、修改以至于难于管理借用关系时。

使用 `RefCell` 时，违背借用规则会导致运行期的 `panic`。

## 与[[Rust/Rc|Rc]]组合使用

A common way to use `RefCell<T>` is in combination with `Rc<T>`. Recall that `Rc<T>` lets you have multiple owners of some data, but it only gives immutable access to that data. If you have an `Rc<T>` holding a `RefCell<T>`, you can get a value with multiple owners _and_ that you can mutate!

```rust
use std::cell::RefCell;
use std::rc::Rc;

fn main() {
    let s = Rc::new(RefCell::new("我很善变，还拥有多个主人".to_string()));

    let s1 = s.clone();
    let s2 = s.clone();
    s2.borrow_mut().push_str(", oh yeah!");

    println!("{:?}\n{:?}\n{:?}", s, s1, s2);
}
```