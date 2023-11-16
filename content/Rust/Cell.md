# Cell

`Cell`智能指针用于提供内部可变性（内部使用了`unsafe`）。

```rust
use std::cell::Cell;

fn main() {
    let c = Cell::new("asdf");
    let one = &c;
    let two = &c;
    one.set("qwer");
    two.set("tyui");
    println!("{:?},{:?}", one, two);
}
```

由于最多只能拥有一个可变引用，使用 `&mut` 无法实现上述逻辑。

被`Cell`的包裹的对象必须实现`Copy`特征。如将上述代码中的`&str`改为`String`类型，即报错。