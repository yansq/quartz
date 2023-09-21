# PriorityQueue

优先级队列

初始化：
```java
Queue<String> PriorityQueue = new PriorityQueue<>();
```

默认按从小到大的顺序排序。

自定义排序规则：
```java
Queue<Integer> PriorityQueue = new PriorityQueue<>(
    (i1, i2) -> i2 - i1);
```

