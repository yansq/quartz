# Arc

`Arc`（`Atomic Rc`），是原子化的[[Rust/Rc|Rc]]智能指针，可用于多线程编程中。

```rust
use std::sync::Arc;
use std::thread;

fn main() {
    let s = Arc::new(String::from("多线程"));
    for _ in 0..10 {
        let s = Arc::clone(&s);
        let handle = thread::spawn(move || {
            println!("{}", s)
        });
    }
}
```