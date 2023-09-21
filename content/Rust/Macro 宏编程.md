# Macro 宏编程

## 宏的分类

- 声明式宏（declarative macros）：`macro_rules!`
- 过程宏（procedural macros）：
    - `#[derive]`：派生宏
    - 类属性宏（Attribute-like macro）：为目标添加自定义属性
    - 类函数宏（Function-like macro）

## 宏和函数的区别

- 元编程：通过一种代码来生成另一种代码
- 可变参数
- 宏展开：宏展开成代码的过程发生在编译器解释代码之前。因此，宏可以为指定的类型实现某些特征。且没有运行期的性能损耗。

## 声明式宏

与`match`类型，但模式匹配的对象是源代码。匹配，传入声明式宏的源代码会被模式关联的代码替换，最终实现宏展开。


使用声明式宏实现支持任意类型的动态数组：

```rust
#[macro_export]
macro_rules! vec {
    ( $( $x:expr ), * ) => {
        {
            let mut temp_vec = Vec::new();
            $(
                temp_vec.push($x);
            )*
            temp_vec
        }
    };
}
```

`#[macro_export]`：导出宏，让其它包可以将宏引入到当前作用域中。
`macro_rules!`：宏定义
`( $( $x:expr ),* )`：宏匹配的源代码模式，类似正则:

- `$()` 中包含的模式是 `$x:expr`，`expr`表示会匹配任何 Rust 表达式，并给予该模式名称`$x`
- `$()`之后的逗号，意味着多个匹配对象间使用逗号分隔
- `*`表示匹配之前的模式任意次

综上，在使用`vec![1, 2, 3]`来调用该宏时，`$x`模式将被匹配3次，分别为1、2、3。宏展开后的代码即为：
```rust
let v = {
    let mut = temp_vec = Vec::new();
    temp_vec.push(1);
    temp_vec.push(2);
    temp_vec.push(3);
    temp_vec
}
```

## 过程宏

当**创建过程宏**时，它的定义必须要放入一个独立的包中。原因在于它必须先被编译后才能使用，如果过程宏和使用它的代码在一个包，就必须先单独对过程宏的代码进行编译，然后再对我们的代码进行编译，但悲剧的是 Rust 的编译单元是包，因此无法做到这一点。


### derive 宏

自定义派生宏：

安装 `cargo-expand`工具：`cargo install cargo-expand`
在`Cargo.toml`中添加：
```rust
[lib]
// 过程宏开开关
proc-macro = true

[dependencies]
syn = "1.0"
quote = "1"
```

在`lib.rs`中：
```rust
extern crate proc_macro;

use proc_macro::TokenStream;
use quote::quote;
use syn;
use syn::DeriveInput;

#[proc_macro_derive(HelloMacro)]
pub fn hello_macro_derive(input: TokenStream) -> TokenStream {
    let ast: DeriveInput = syn::parse(input).unwrap();
    impl_hello_macro(&ast)
}

fn impl_hello_macro(ast: &syn::DeriveInput) -> TokenStream {
    let name = &ast.ident;
    let gen = quote!{
        impl HelloMacro for #name {
            fn hello_macro() {
                println!("Hello, my name is {}", stringify!(#name));
            }
        }
    };
    gen.into()
}
```

`TokenStream`在`proc_macro`包中定义，代表一个 `Token`序列。
`syn` 将字符串形式的 Rust 代码解析为一个 AST 树（Abstract Syntax Tree）的数据结构。

`derive`只能用于结构体和枚举。

### 类属性宏

类属性宏与`derive`宏类似，但可用于其他类型，例如函数；并且允许自定义属性，如：
```rust
#[route(GET, "/")]
fn index() {}
```

### 类函数宏

定义可以像函数那样调用的宏，类似声明宏，但可处理更复杂的逻辑

```rust
#[proc_macro]
pub fn sql(input: TokenStream) -> TokenStream {
    // ...
}

// 使用
let sql = sql!(SELECT * FROM dual where id = 1);
```