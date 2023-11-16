# Rust 可见性

- `fn test()`：私有
- `pub fn test()`：对 module 可见
- `pub(crate) fn test()`：crate 内可见
- `pub(super)`：对父模块可见
- `pub(in path)`：对 path 可见