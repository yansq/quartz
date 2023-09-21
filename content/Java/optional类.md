# Optional 类

## 作用

首次出现在 Google 的 [[Guava]] 类库中，在 Java 8 中引入，用于解决 `NullPointerException` 问题，避免代码中频繁的 null 检查。

## 数据结构

Optional 类是对原有数据类型的一个**封装**。如果原有变量不存在，Optional 类会通过 `Optional.empty()` 方法返回空的 Optional 对象，避免返回 null 值。

## 使用

### 创建

创建空 Optional：
```java
Optional<Car> option = Optional.empty();
```

创建非空 Optional：
```java
Optional<Car> option = Optional.of(myCar);
```

创建可以包含 null 值的 Optional：
```java
Optional<Car> option = Optional.ofNullable(yourCar);
```

### 判断值/获取值

判断是否存在值 `isPresent()`，获取原数据 `get()` ：
```java
if(option.isPresent()) {
    System.out.println(option.get());
}
```

`ifPresent` 会接收一个消费者类型[[函数式接口]]的实现，如果 Optional 实例中的值不为空，则调用 Cosumer 的 accept 方法对 value 进行消费，若值为空则不做处理：
```java
public void ifPresent(Consumer<? super T> action) {
    if(value != null) {
        action.accept(value);
    }
}
```

使用 `ifPresent` 来代替 `isPresent()` 与 `get()` 方法：
```java
optional.isPresent((val) -> System.out.println(val));
```

`orElse()` ： 同 `get()` 方法，在值为空时返回默认值
```java
Optional<Car> option = Optional.empty();
System.out.println(option.orElse("MINI COOPER");
```

`orElseGet()` ：接收一个 `Supplier` 类型的 [[函数式接口]] 实现，功能同 ` orElse ()` 
```java
String  Optional.empty().orElseGet(() -> "Default");
```

`orElseThrow()` ：接收 [[函数式接口]], 如无值抛出异常

`map` ：使用 Lambda 表达式对 Optional 对象进行操作，map 函数返回前会封装为 Optional 对象。
使用 Stream 的 map：
```java
public Optional<Employee> findEmployeeById(int id) {
    // do something
}

public List<Employee> findEmplogyeeByIds(List<Integer> ids) {
    return ids.stream()
        .map(this::findEmployeeById)
        .filter(Optional::isPresent)
        .map(Optional::get)
        .collect(Collectors.toList());
}
```

使用 Optional 的 map：
```java
public List<Employee> findEmplogyeeByIds(List<Integer> ids) {
    return ids.stream()
        .map(this::findEmployeeById)
        .flatMap(optional -> optional.map(Stream::of)
                                     .orElseGet(Stream::empty))
        .collect(Collector.toList());
}
```

`flatMap` ：与 `map` 类似，区别在与 flatMap 的返回值必须是 Optional，不会像 `map` 那样进行封装。

`filter` : 对 Optional 对象进行校验，如果满足条件返回原对象，如果不满足返回空 Optional 对象。
```java
Optional<String> longName = username.filter((value) -> value.length() > 2); 
System.out.println(longName.orElse("The name is less than 2 characters"));
```

## 获取 optional 对象的属性值
```java
Optional<Person> person = getPerson(index);
return person.map(Person::getName).orElse("unknown");
```

## 场景

使用 `Optional` 的目的是为了上开发者不要忘记非空检查。

Optional 类的缺点有：
- 不能避免 NPE 问题，只是将异常转化为 `NoSuchElementException`
- 更难理解
- 效率较低

[[阿里《Java开发手册》]]建议对于级联调用 `obj.getA().getB().getC()` 使用 Optional 类来防止 NPE 问题。如果在不使用链式语法的场景，使用 null 来判断效率更高。

尽量避免将 Optional 用于类属性，方法参数与集合元素中，因为完全可以使用 null, 违反了使用 Optional 类的初衷。

## 参考资料

- [Java进阶篇（3）—Optional类（是否使用Optional来代替null）](https://www.jianshu.com/p/ef242efeaabd)
- [讲讲Java8的Optional类](https://segmentfault.com/a/1190000038471657)]
- [Java Optional的映射](https://geek-docs.com/java/java-examples/java-optional-mapping.html)