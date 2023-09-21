# list 转 map

## 使用 guava

```java
List<String> list = new ArrayList<>();  
list.add("aaa");  
list.add("bbb");  
list.add("ccc");  
Map<String, String> map = Maps.uniqueIndex(list, (item) -> "index" + item);
```

## 使用 Stream

```java
List<String> list = new ArrayList<>();  
list.add("aaa");  
list.add("bbb");  
list.add("ccc");
Map<String, String> map2 = 
list.stream().collect(Collectors.toMap((item) -> "index" + item  
        , Function.identity()));
```

其中 `Function.identity()`，基本等同于`t -> t`。