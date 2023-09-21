# fastjson 反序列化最好设置无参构造函数

## 场景

在使用 fastjson 的 `parseObject()` 方法反序列化 json，目标对象大概长这样：

```java
@Data
public class TestClass() {
    private String id;
    private String name;

    public TestClass(String name) {
        this.id = UUID.randomUUID().toString();
        this.name = name;
    }
}
```

对象中得到的 id 与 jsonstring 中的 id 不同。

## 原理

fastjson 在反序列化时在没有无参构造的情况下，默认会去找参数最多的构造函数，导致 id 被重写

## 解决方案

加入无参构造

```java
    public TestClass() {}
```

