# HashMap

Init:
```rust
use std::collections::HasMap;

let mut my_gems = HashMap::new();

my_gems.insert("Ruby", 1);
my_gems.insert("Obsidian", 2);
my_gems.insert("Stone", 18);
```

Init hashmap with capacity：
```rust
let capacity = 10;
let hash_map = HashMap::with_capacity(capacity);
```

Vector to Hashmap:
```rust
use std::collections::HashMap;
let teams_list = vec![ 
    ("中国队".to_string(), 100), 
    ("美国队".to_string(), 10), 
    ("日本队".to_string(), 50), 
];
let teams_map: HashMap<_,_> = tems_list.into_iter().collect();
println!("{:?}",teams_map)
```

Get elements from Hashmap:
```rust
let mut scores = HashMap::new();
scores.insert(String::from("Blue"), 10);
scores.insert(String::from("Yellow"), 50);

let team_name = String::from("Blue");
let score: Option<&i32> = scores.get(&tema_name);
```

If we want to get value directly, instead of `Option`:
```rust
let score: i32 = scores.get(&team_name).copied().unwrap_or(0);
// or
if scores.contains_key("Blue") {
    let value = scores["Blue"];
}
```

For-loop:
```rust
for (key, value) in &scores {
    println!("{}: {}", key, value);
}
```

Update:
```rust
fn main() {
    use std::collections::HashMap;

    let mut scores = HashMap::new();
    scores.insert("Blue", 10);

    // 覆盖已有的值
    let old = scores.insert("Blue", 20);
    assert_eq!(old, Some(10));

    // 查询Yellow对应的值，若不存在则插入新值
    let v = scores.entry("Yellow").or_insert(5);
    assert_eq!(*v, 5); // 不存在，插入5

    // 查询Yellow对应的值，若不存在则插入新值
    let v = scores.entry("Yellow").or_insert(50);
    assert_eq!(*v, 5); // 已经存在，因此50没有插入
}
```