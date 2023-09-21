# Cargo

Rust 的包管理器。

进行生产编译：`cargo build --release`（默认是Debug 模式，生产模式的性能会更好）

## 查看包

```
cargo search structopt
```

## 指定依赖项

- 1.2.3.:= 1.2.3
- ^1.2.3 := >=1.2.3, <2.0.0
- ~1.2.3 := >=1.2.3, <1.3.0
- 1.2.* := >=1.2.0, <1.3.0
- >= 1.2.0, < 1.3.0 := >=1.2.0 <1.3.0

## 更新依赖

`cargo update`：更新所有依赖
`cargo update -p regex`：更新单个依赖

## 标准 Package 目录结构

```
.
├── Cargo.lock
├── Cargo.toml
├── src/
│   ├── lib.rs
│   ├── main.rs
│   └── bin/
│       ├── named-executable.rs
│       ├── another-executable.rs
│       └── multi-file-executable/
│           ├── main.rs
│           └── some_module.rs
├── benches/
│   ├── large-input.rs
│   └── multi-file-bench/
│       ├── main.rs
│       └── bench_module.rs
├── examples/
│   ├── simple.rs
│   └── multi-file-example/
│       ├── main.rs
│       └── ex_module.rs
└── tests/
    ├── some-integration-tests.rs
    └── multi-file-test/
        ├── main.rs
        └── test_module.rs
```

## 文档

- 生成文档：`cargo doc`
- 打开文档：`cargo doc --open`

