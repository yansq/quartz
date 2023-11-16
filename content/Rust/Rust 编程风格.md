# Rust 编程风格

- 函数、变量、模块蛇形命名的，如`time_in_millis`；
- 结构体驼峰式，如`CpuModel`；
- 常量大写的蛇形命名，如`GLOBAL_TIMEOUT`

如果需要重复使用变量名，可以对第一个变量的末尾加`_`，如：
```rust
for line_ in reader.lines() {
    let line = line_.unwrap();
    println!("{} ({} bytes long)", line, line.len());
}
```

---

针对可能为null的值，使用 option 初始化：
```rust
let five = Some(5);
```

---

hashmap，当 value 不存在时设置默认值并插入：
```rust
let count = hashmap.entry(String::from("key")).or_insert(0);
*count += 1;
```

