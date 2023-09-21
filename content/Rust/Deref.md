# Deref

智能指针实现了`Deref`特征。`Deref`可以让智能指针像引用那样工作，这样就可以同时支持智能指针和引用的代码。

一个类型为`T`的对象`foo`，如果`T:Deref<Target=U>`，相关`foo`的引用`&foo`在应用的时候会自动转换为`&U`。

```rust
use std::ops::Deref;

struct MyBox<T>(T);

impl<T> MyBox<T> {
    fn new(x: T) -> MyBox<T> {
        MyBox(x)
    }
}

impl<T> Deref for MyBox<T> {
    type Target = T;
    fn deref(&self) -> &Self::Target {
        &self.0
    }
}

fn test(a: &i32) {
    println!("{}", *a);
}

fn main() {
    let y = MyBox::new(t);
    // &y: &MyBox<i32>
    test(&y);
}
```

`test(&y)`传入的是`&MyBox<i32>`，被自动转换为了`&i32`。

---

在实现了`Deref`特征之后，就可以对智能指针进行解引用操作。

```rust
fn main() {
    let y = MyBox::new(5);
    assert_eq!(5, *y);
}
```

当对智能指针进行解引用时，实际上调用了`*(y.deref())`。

---

隐式 Deref 转换

```rust
fn main() {
    let s = String::from("hello world");
    display(&s)
}

fn display(s: &str) {
    println!("{}", s);
}
```

`String`是一个智能指针，实现了`Deref`特征。在以上代码中将变量`s`进行了隐式转换，由`&String`类型转换为了`&str`（字符串切片）类型。