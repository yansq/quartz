# Mutex

互斥锁

```rust
use std::sync::Mutex;

fn main() {
    let m = Mutex::new(5);
    {
        let mut num = m.lock().unwrap();
        *num = 6;
        // lock drop automatically
    }
    pritnln!("m = {:?}", m);
}
```

`Mutex<T>`是一个智能指针，实现了[[Rust/Deref|Deref]]特征，因此`num`能够直接获取数据；实现了[[Rust/Drop|Drop]]特征，因此在超出作用域后会自动释放。

在多线程中使用互斥锁：

```rust
use std::sync::{Arc, Mutex};
use std::thread;

fn main() {
    let counter = Arc::new(Mutex::new(0));
    let mut handles = vec![];

    for _ in 0..10 {
        let counter = Arc::clone(&counter);
        let handle = thread::spawn(move || {
            let mut num = counter.lock().unwrap();
            *num += 1;
        });
        handles.push(handle);
    }

    for handle in handles {
        handle.join().unwrap();
    }

    println!("Result: {}", *counter.lock().unwrap());
}
```

[[Rust/Rc|Rc]]没有实现 `Send` 特征，不能在多线程中传递。
需要使用 `Arc` ，`Arc`的内部计数器是线程安全的。

[[Rust/Rc|Rc<T>]] / [[Rust/RefCell|RefCell<T>]]用于单线程内部可变性， `Arc<T> / Mutex<T>`用于多线程内部可变性。

`try_lock`会尝试去获取一次锁，如果无法获取会返回一个错误，使用`try_lock`代替`lock`可以避免阻塞。

