# turbofish

```rust
fn main() {
    let numbers: Vec<i32> = vec![
        1, 2, 3, 4, 5, 6, 7, 8, 9, 10,
    ];

    let even_numbers = numbers
        .into_iter()
        .filter(|n| n % 2 == 0)
        .collect();

    println!("{:?}", even_numbers);
}
```

以上代码会报错，因为编译器并不知道 `even_numbers` 的类型。

为解决这个问题，可以直接指定 `even_numbers` 的类型：
`let even_number: Vec<i32> = ...`

或是使用 turbofish 语法：
```rust
let even_numbers = numbers
    .into_iter()
    .filter(|n| n % 2 == 0)
    .collect::<Vec<i32>>();
```

`collect()`的函数签名为：`fn collect<B>(self) -> B`。使用 turbofish  `::<>` 相当于指定了泛型`<B>`的类型。







