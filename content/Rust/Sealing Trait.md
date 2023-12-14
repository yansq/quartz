# Sealing Trait

## Sealing traits with a supertrait(best practice)

```rust
mod private {
    pub trait Sealed {}
}

pub trait SealedTrait : private::Sealed {
    fn method(&self);
}
```

```rust
pub struct TypeThatImplsSealed;

impl private::Sealed for TypeThatImplsSealed {}

impl SealedTrait for TypeThatImplsSealed {
    fn method(&self) {}
}
```

 Sealing the trait in this way doesn't prevent downstream code from calling its methods. The following code in a downstream crate works just fine:
```rust
fn use_sealed(value: impl upstream::SealedTrait) {
    value.method()
}
```

## Sealing traits via method signatures

```rust
mod private {
    pub struct Token;
}

pub trait SealedTrait {
    fn method(&self, _: private::Token);
}
```

```rust
pub struct TypeThatImplsSealed;

impl SealedTrait for TypeThatImplsSealed {
    fn method(&self, _: private::Token) {
        // impl here
    }
}
```

Meanwhile, downstream code can both see and name the trait and its method, but cannot implement the trait nor call the method.

## References

- [A definitive guide to sealed traits in Rust](https://predr.ag/blog/definitive-guide-to-sealed-traits-in-rust/)