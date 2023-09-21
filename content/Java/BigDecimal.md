在需要进行精确的浮点数计算时使用，通常用于金额计算。

当初始化一个 BigDecimal 时，**不要使用** Double 入参的构造方法，会存在精度丢失问题：
```java
BigDecimal bd = new BigDecimal(4.015);
```

为解决这个问题，可以考虑将 Double 先转换为 String，再进行传入：
```java
BigDecimal bd = new BigDecimal(String.valueOf(4.015));
```

或者使用`BigDecimal.valueOf()`方法：
```java
BigDecimal bd = BigDecimal.valueOf(4.015);
```

