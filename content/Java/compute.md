# compute

操作 map 的简化写法

```java
Map<String, Integer> map = new HashMap<>();
// ...
if (map.contains(key)) {
    map.put(key, map.get(key) + value);
} else {
    map.put(key, value);
}
```

可以简化为：

```java
Map<String, Integer> map = new HashMap<>();
// ...
map.compute(key, (k, v) -> v == null ? value : v + value)
```