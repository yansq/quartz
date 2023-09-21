# Box

`Box<T>`是一个智能指针，可以将值分配到堆上，然后在栈上保留一个智能指针，指向堆上的数据。

使用场景：

- 特意将数据分配在堆上
- 数据较大，并且不想在转移所有权时进行数据拷贝
- 类型的大小在编译期无法确定，但又需要固定大小的类型
- 特征对象，用于说明对象实现了一个特征，而不是某个特定的类型

## 特意将数据分配在堆上

```rust
fn main() {
    let a = Box::new(3);
    println!("a = {}", a);
}
```

`println!`可以正常打印出`a`的值，是因为它隐式调用了`Deref`对智能指针`a`进行了解引用。

`a`持有 的智能指针将在作用域结束（`main`函数结束）时，被释放，因为`Box<T>`实现了`Drop`特征。

## 避免栈上数据的拷贝

```rust
fn main() {
    // 在栈上创建一个长度为1000的数组
    let arr = [0;1000];
    // 由于 arr 分配在栈上，此时是将 arr 的数据拷贝了一份给 arr1
    let arr1 = arr;

    // 使用 Box 包裹数组
    let arr = Box::new([0;1000]);
    // 此时进行了所有权转移，底层数据没有被拷贝
    let arr1 = arr;
    // arr 失去所有权，该语句报错
    println!("{:?}", arr.len());
}
```

## 将动态大小类型变为固定大小类型

递归类型无法在编译时得知大小：
```rust
enum List {
    Cons(i32, List),
    Nil,
}
```

此时使用`Box<T>`可避免报错：
```rust
enum List {
    Cons(i32, Box<List>),
    Nil,
}
```

## 包裹特征对象

```rust
trait Draw {
    fn draw(&self);
}

struct Button {
    id: u32,
}

impl Draw for Button {
    fn draw(&self) {
        println!("这是屏幕上第{}号按钮", self.id)
    }
}

struct Select {
    id: u32,
}

impl Draw for Select {
    fn draw(&self) {
        println!("这个选择框贼难用{}", self.id)
    }
}

fn main() {
    let elems: Vec<Box<dyn Draw>> = vec![
        Box::new(Button { id: 1 }), 
        Box::new(Select { id: 2 })
    ];

    for e in elems {
        e.draw()
    }
}
```

## Box::leak

`Box::leak`用于消费掉`Box`，并且强制目标值从内存中泄漏。

例如，可以把一个 `String` 类型，变成一个 `'static` 生命周期的 `&str` 类型：

```rust
fn main() {
   let s = gen_static_str();
   println!("{}", s);
}

fn gen_static_str() -> &'static str{
    let mut s = String::new();
    s.push_str("hello, world");

    Box::leak(s.into_boxed_str())
}
```

以上代码将一个在运行期初始化的值`s`，设置为全局有效，也就是和整个程序活得一样久。