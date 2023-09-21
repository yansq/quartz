
hashmap 有以下几种遍历方式：

1. 使用迭代器（[[Iterator]]）EntrySet
2. 使用迭代器（[[Iterator]]）KeySet
3. 使用 `For Each EntrySet`
4. 使用 `For Each KeySet`
5. 使用 Lambda 表达式
6. 使用 Streams APi（单线程 / 多线程）

### 使用 EntrySet

```java
Iterator<Map.Entry<Key.class, Value.class>> iterator = hashMap.entrySet().iterator();
while (iterator.hasNext()) {
    Map.Entry<Key.class, Value.class> entry = iterator.next();
    System.out.println(entry.getKey() + ": " + entry.getValue());
}
```

### 使用 KeySet

```java
Iterator<Map.Entry> iterator = hashMap.keySet().iterator();
while (iterator.hasNext()) {
    String key = iterator.next();
    System.out.println(key + ": " + map.get(key));
}
```

### EntrySet 和 KeySet 的性能比较

`EntrySet` 的性能好于 `KeySet`，因为`KeySet`要多一次`get()`的操作。

### ForEach EntrySet

```java
for (Map.Entry<Key.calss, Value.class> entry : map.entrySet()) {
    System.out.println(entry.getKey() + ": " + entry.getValue());
}
```

### ForeEach KeySet

```java
for (String key : map.keySet()) {
    System.out.println(key + ": " + map.get(key));
}
```

### Lambda

```java
map.forEach((key, value) -> {
    System.out.println(key + ": " + value);    
});
```

### Streams API 单线程

```java
map.entrySet().stream().forEach((entry) -> {
    System.out.println(entry.getKey() + ": " + entry.getValue());
})
```

### Streams API 多线程

```java
map.entrySet().parallelStream().forEach((entry) -> {
    System.out.println(entry.getKey() + ": " + entry.getValue());
})
```
