# tokio

tokio 是 Rust 的一个异步运行时。

## tokio::main 宏

Rust 自身不提供异步运行时，因此在声明 `async` 函数时需要指定运行时。在 tokio 中，使用 `#\[tokio\::main\]` [[Rust/Macro 宏编程|宏]] 指定运行时。

```rust
#[tokio::main]
async fn main() {
    println!("hello");
}
```

上述代码会被宏展开为：

```rust
fn main() {
    let mut rt = tokio::runtime::Runtime::new().unwrap();
    rt.block_on(async {
        println!("hello");
    })
}
```

## tokio::spawn

使用 `tokio::spawn` 可以创建一个 Tokio 任务，传入参数为一个 `async` 语句块，返回一个 `JoinHandle`。对 handle 使用 `.await` 可以获取 `async` 语句块的返回值。

```rust
#[tokio::main]
async fn main() {
    let handle = tokio::spawn(async {
        "return value"
    });
    let out = handle.await.unwrap();
    println!("GOT {}", out);
}
```

Tokio 任务交由 Tokio 调度器进行调度。任务执行的线程可能与创建任务的线程相同，也可能不同。

Task 的[[Rust/生命周期|生命周期]]为`'static`，这意味着如果直接在 `async` 语句块中使用外部变量，编译器会报错，需要使用 [[Rust/闭包#move|move]] 获取变量的所有权。

```rust
use tokio::task; 
#[tokio::main] 
async fn main() { 
    let v = vec![1, 2, 3]; 
    task::spawn(move async { 
        println!("Here's a vec: {:?}", v); 
    }); 
}
```

## Send 特征

如果在执行 `await`时，`async` 块中仍然有变量处于作用域中，那么该变量必须实现 `Send` 特征。这样  Tokio 调度器才能将任务在线程间安全地移动。

```rust
use tokio::task::yield_now;
use std::rc::Rc;

#[tokio::main]
async fn main() {
    tokio::spawn(async {
        let rc = Rc::new("hello");
        yield_now().await;
        // rc 作用域在这里结束
    });
}
```

上述代码会报错，在执行`await`时，变量 `rc` 仍在其作用域中。