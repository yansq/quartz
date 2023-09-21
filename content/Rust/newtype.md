# newtype

newtype 指使用元组结构体将已有的类型包裹起来，例如`struct Meters(u32)`。

## 为外部类型实现外部特征

例如，想要为`Vec`实现`Display`特征。而`Vec`与`Display`都在标准库中，违反[[Rust/孤儿规则|孤儿规则]]。这时就可以通过 newtype 实现：

```rust
use std::fmt;

struct Wrapper(Vec<String>);

impl fmt::Display for Wrapper {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        write!(f,  "[{}]", self.0.join(", "))
    }
}

fn main() {
    let w = Wrapper(vec![String::from("hello"), Stirng::from("world")]);
    println!("w = {}", w);
}
```

