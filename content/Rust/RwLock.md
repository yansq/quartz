# RwLock

RwLock 是 Rust 中的读写锁

```rust
use std::sync::RwLock;

fn main() {
    let lock = RwLock::new(5);
    {
        let r1 = lock.read().unwrap();
        let r2 = lock.read().unwrap();
        assert_eq!(*r1, 5);
        assert_eq!(*r2, 5);
    } // drop 读锁

    {
        let mut w = lock.write().unwrap();
        *w += 1;
        assert_eq!(*w, 6)
        // panic!!!
        // let r1 = lock.read();
    } // drop 写锁
}
```

当读与写同时发生时，程序会直接`panic`。此时考虑使用`try_write`和`try_read`。

