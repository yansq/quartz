# Vector

可变长度的[[Rust/数组|数组]]，数据存储在堆上，因此可以动态改变容量。由指针、长度、容量三部分构成。

```rust
let mut v1 = Vec::new();
v1.push(1);
v1.push(2);

let v2 = vec![1,2];

// [3,3,3,3]
let v3 = vec![3;4]
```

Vector 配置特征对象使用：

```rust
trait Bird {
    fn quack(&self);
}

struct Duck;
impl Duck {
    fn fly(&self) {
        println!("Look, the duck is flying")
    }
}
struct Swan;
impl Swan {
    fn fly(&self) {
        println!("Look, the duck.. oh sorry, the swan is flying")
    }
}

impl Bird for Duck {
    fn quack(&self) {
        println!("{}", "duck duck")
    }
}

impl Bird for Swan {
    fn quack(&self) {
        println!("{}", "swan swan")
    }
}

fn main() {
    let birds: Vec<Box<dyn Bird>> = vec![Box::new(Duck{}), Box::new(Swan{})];

    for bird in birds {
        bird.quack();
    }
}
```

get element from Vector:

```rust
let v = vec![1, 2, 3, 4, 5];

let third: &i32 = &v[2];
println!("The third element is {}", third)

// vec.get() returns Option<&T>
match v.get(2) {
    Some(third) => println!("The third element is {third}"),
    None => println!("There is no third element"),
}
```

sort a vector:

- stable sort:  `sort` and `sort_by`
- unstable sort: `sort_unstable` and `sort_unstable_by`

```rust
fn main() {
    let mut vec = vec![1, 5, 10, 2, 15];
    vec.sort_unstable(); 
    assert_eq!(vec, vec![1, 2, 5, 10, 15]);
}
```

sort vector with float elements or struct elements:

```rust
fn main() {
    let mut vec = vec![1.0, 5.6, 10.3, 2.0, 15f32];    
    vec.sort_unstable_by(|a, b| a.partial_cmp(b).unwrap());    
    assert_eq!(vec, vec![1.0, 2.0, 5.6, 10.3, 15f32]);
}
```

```rust
#[derive(Debug)]
struct Person {
    name: String,
    age: u32
}

impl Person {
    fn new(name: String, age: u32) -> Person {
        Person { name, age }
    }
}

fn main() {
    let mut people = vec![
        Person::new("Zoe".to_string(), 25),
        Person::new("Al".to_string(), 60),
        Person::new("John".to_string(), 1),
    ];
    people.sort_unstable_by(|a, b| b.age.cmp(&a.age));
    println!("{:?}", people)
}
```