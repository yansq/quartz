# Async & Await

Rust 同时支持[[多线程]]和 Async / Await 两套并发模型。

`Async`底层也是基于线程实现，但基于线程封装了一个运行时，可以将多个任务映射到少量线程上，将线程切换转换为任务切换，提高效率。

- 对于 CPU 密集型场景：多线程会更有优势，性能更高
- 对于 IO 密集型场景：使用 Async 更好，因为使用多线程会有大量线程闲置，开销高。

由于编译器会为`async`生成状态机，然后将整个运行时打包进来，会导致编译出的二进制可执行文件体积显著增大。

---

Rust 的 `Future` 是**惰性**的，并不像其他一些语言一样，创建后就会自动执行。其中一个推动它的方式是在`async` 函数中使用 `.await` 来调用另一个 `async` 函数。当 `await` 被调用时，它会尝试运行 `Future` 直到完成，若 `Future` 进入阻塞，就会让出当前线程的控制权。当 `Future` 准备再一次被运行时，执行器会得到通知，并再次运行该 `Future`，如此循环，直到完成。

最外层的 `async` 函数只能由执行器 `executor` 来推动。 

---

`aysnc` 函数返回的 `Future` 默认实现了 [[Rust/Pin & Unpin#!Unpin|!Unpin]] 特征。将 `Pin` 住的 `Future` 转换为 [[Rust/Pin & Unpin#Unpin|Unpin]]的方法：

```rust
use pin_utils::pin_mut; // `pin_utils` 可以在crates.io中找到

// 函数的参数是一个`Future`，但是要求该`Future`实现`Unpin`
fn execute_unpin_future(x: impl Future<Output = ()> + Unpin) { /* ... */ }

let fut = async { /* ... */ };
// 下面代码报错: 默认情况下，`fut` 实现的是`!Unpin`，并没有实现`Unpin`
// execute_unpin_future(fut);

// 使用`Box`进行固定
let fut = async { /* ... */ };
let fut = Box::pin(fut);
execute_unpin_future(fut); // OK

// 使用`pin_mut!`进行固定
let fut = async { /* ... */ };
pin_mut!(fut);
execute_unpin_future(fut); // OK
```