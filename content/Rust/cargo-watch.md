# cargo-watch

监测代码变换，当发生变化时执行对应的 cargo 命令，以减少编译时间。

## 安装

```shell
cargo install cargo-watch
```

## 使用

```shell
cargo watch -x check
```

当代码变更时，执行 `cargo check` 命令。

```shell
cargo watch -x check -x test -x run
```

链式执行 `cargo check` 、`cargo test` 与 `cargo run` 命令。