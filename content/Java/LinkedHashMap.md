# LinkedHashMap

  可以维护元素顺序的散列表。
  
  初始化：
```java
Map<String, String> map = new LinkedHashMap<String, String>(16,0.75f,true);
```

- 第一个参数：初始长度
- 第二个参数：装载因子
- 第三个参数：是否根据访问顺序排序

LinkedHashMap继承自HashMap,它的新增(put)和获取(get)方法都是复用父类HashMap的代码，**只是重写了put给get内部的某些接口**。