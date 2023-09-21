# Cow

```rust
pub enum Cow<'a, B> 
where
    B: 'a + ToOwned + ?Sized, 
{
    Borrowed(&'a B),
    Owned(<B as ToOwned>::Owned),
}
```

用于实现写时克隆（Clone on write）的智能指针，包含一个对类型 B 的只读引用，或者包含一个拥有类型 B 的所有权的数据。 使用 Cow 使我们在返回数据时提供了两种可能: 要么返回一个借用的只读数据（`Borrowed`），要么返回一个拥有所有权的数据（`Owned`）。

类型 B 需满足以下特征：

- ToOwned：表示可以把借用的B数据克隆出一个拥有所有权的数据
- ?Sized：表示B是可变大小类型

`<B as ToOwned>::Owned`：将 B 强制转换为 `ToOwned`类型，然后返回 `Owned`。

Cow 方法：

- `to_mut()`：可以得到一个具有所有权的可变引用，多次调用只会产生一次克隆
- `into_owned()`：可以得到具有所有权的值。如果之前是`Borrowed`状态，调用`into_owned()`会触发克隆，`Owned`状态不会。

使用场景：

在读多写少的场景，等到写时再进行复制，减少内存复制次数。

```rust
use std::borrow::Cow;

fn main() {
    fn abs_all(input: &mut Cow<[i32]>) {
        for i in 0..input.len() {
            let v = input[i];
            if v < 0 {
                // Clones into a vector if not already owned.
                input.to_mut()[i] = -v;
            }
        }
    }

    // No clone occurs because input doesn't need to be mutated.
    let slice = [0, 1, 2];
    let mut input = Cow::from(&slice[..]);
    abs_all(&mut input);

    // Clone occurs because input needs to be mutated.
    let slice = [-1, 0, 1];
    let mut input = Cow::from(&slice[..]);
    abs_all(&mut input);

    // No clone occurs because input is already owned.
    let mut input = Cow::from(vec![-1, 0, 1]);
    abs_all(&mut input);
}
```