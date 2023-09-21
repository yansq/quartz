
将`List<List<String>>`中的所有字符串放到一个`List<String>`：

```java
public static List<String> transform(List<List<String>> collection) {

    return collection
        .stream()
        .flatMap(l -> l.stream())
        .collect(Collectors.toList());

}
```


---

获取`List<Person>`中，年龄最大的Person对象：

```java
List<Person> list = new ArrayList<>();
// add data

// get 是将 Optional 对象转换为原始对象
Person oldestPerson = list.stream()
                          .reduce((p1, p2) -> 
                                  p1.getAge() > p2.getAge() ? p1 : p2)
                          .get();        
```

或者

```java
Person oldestPerson = list.stream()
                          .max(Comparator.comparingInt(Person::getAge));
```

---

对`List<Integer>`求和：

```java
List<Integer> list = new ArrayList<>();
// add data

int sum = list.stream()
              .mapToInt(Ingeter::intValue)
              .sum();
              
```

或者

```java
int sum = list.stream().reduce(0, Integer::sum);
```

---

输入数据`List<Person>`，根据Person的年龄分组，返回map对象：

```java
List<Person> list = new ArrayList<>();
// add data

Map<Integer, Person> map = list.stream()
                            .collect(Collectors.groupingBy(s -> s.getAge()));
```

---

拼接字符串（各种操作）：

```java
public static String namesToString(List<Person> people) {

    // Names: name1, name2, name3.
    return people.stream() 
            .map(Person::getName) 
            .collect(joining(", ", "Names: ", "."));
}
```

---

Stream 转数组：

```java
Stream<Person> stream;
Person[] people = stream.toArray(Person[]::new);
```
