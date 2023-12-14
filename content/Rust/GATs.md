# GATs 泛型关联类型

GATs(generic associated types) allows types in trait to be generic.

In some reason, you wnat to let `map()` method returns  `Option<U>` or `Result<U>`.

Declare trait:
```rust
trait Mappable {
    type Item;
    type Result<U>;
    fn map<U, P: FnMut(Self::Item) -> U>(self, f: P) 
        -> Self::Result<U>;
}
```

Implement it for both `Option` and `Result`:
```rust
impl<T> Mappable for Option<T> {
    type Item = T;
    type Result<U> = Option<U>;
    fn map<U, P: FnMut(Self::Item) -> U>(self, f: P) -> Option<U> {
        self.map(f)
    }
}

impl<T, E> Mappable for Result<T, E> {
    type Item = T;
    type Result<U> = Result<U, E>;
    fn map<U, P: FnMut(Self::Item) -> U>(self, f: P) -> Result<U, E> {
        self.map(f)
    }
}
```

Finally, you get the ability to implemnet function which is **generic over generic** like this:
```rust
fn zero_to_42<C: Mappable<Item = i32>>(c: C) -> <C as Mappable>::Result<i32> {
    c.map(|x| if x == 0 { 42 } else { x })
}
```

## References

- [Could someone explain the GATs like I was 5? : r/rust](https://www.reddit.com/r/rust/comments/ynvm8a/could_someone_explain_the_gats_like_i_was_5/)
- [使用Rust GATs改进代码和应用程序性能](https://mp.weixin.qq.com/s/ujARMqa3rcdBbFVEvd6kKg)