# Junit 代码示例

## assertEquals

```java
@Test  
@DisplayName("Simple multiplication should work")  
public void testMultiply() {  
    assertEquals(20, calculator.multiply(4, 5),  
    "Regular multiplication should work");  
}
```

## assertTrue

```java
assertTrue('a' < 'b', () -> "optional failure message");
```

## assertFalse

```java
assertFalse('a' > 'b', () -> "optional failure message");
```

## assertNotNull

```java
assertNotNull(yourObject, "optional failure message");
```

## assertNull

```java
assertNull(yourObject, "optional failure message");
```

## Test for exceptions

```java
@Test 
void ensureThatFellowsStayASmallGroup() { 
    List<TolkienCharacter> fellowship = dataService.getFellowship(); 
    assertThrows(IndexOutOfBoundsException.class, () -> fellowship.get(20)); 
}
```

或是

```java
@Test  
@DisplayName("Set age with negative number")  
public void testSetAgeWithNegativeNumber() {  
    Throwable exception = assertThrows(IllegalArgumentException.class,  
        () -> calculator.setAge(-1));  
    assertEquals("Age must be a positive number", exception.getMessage());  
}
```



## assertAll

```java
@Test 
void groupedAssertions() { 
    Address address = new Address(); 
    assertAll("address name", 
              () -> assertEquals("John", address.getFirstName()), 
              () -> assertEquals("User", address.getLastName()) 
    ); 
}
```

与不用 `assertAll`， 直接写两个 ` assertEquals` 的区别
```java
@Test 
void groupedAssertions() { 
    Address address = new Address(); 
    assertEquals("John", address.getFirstName());
    assertEquals("User", address.getLastName()); 
}
```

不用 `assertAll`，如果执行到一个断言判断失败的时候，就会停止执行之后的测试代码；使用 `assertAll`，可以执行方法中所有断言，无论成功失败。

## timeout

判断方法执行是否超时：

```java
@Test  
@DisplayName("Method's execution time should be less then 1 second")  
public void testMultiplyExecuteTime() {  
    int result = assertTimeout(Duration.ofSeconds(1), 
        () -> calculator.multiply(42424, 24));  
    assertEquals(1018176, result);  
}
```

另有 `assertTimeoutPreemptively` 方法：如果超时了，方法还没有返回，则立即中断执行。

## 关闭测试

### 方案一 @Disabled注解：

使用 `@Disabled || @Disabled("msg")` 注解关闭关闭测试。

### 方案二 assumeFalse 判断：

```java
Assumptions.assumeFalse(System.getProperty("os.name").contains("Linux"));
```

可以使用 `assumeFalse` 方法对条件进行判断，如果参数为 `true`，则不执行下面测试。

## 动态检查（Dynamic Test）

`@TestFactory` 方法必须返回 `DynamicNode` 实例的 `Stream`，`Collection`，`Iterable` 或 `Iterator`。

```java
import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.junit.jupiter.api.DynamicTest.dynamicTest;

import java.util.Arrays;
import java.util.stream.Stream;

import org.junit.jupiter.api.DynamicTest;
import org.junit.jupiter.api.TestFactory;

class DynamicTestCreationTest {

    @TestFactory
    Stream<DynamicTest> testDifferentMultiplyOperations() {
        MyClass tester = new MyClass();
        int[][] data = new int[][] { { 1, 2, 2 }, { 5, 3, 15 }, { 121, 4, 484 } };
        return Arrays.stream(data).map(entry -> {
            int m1 = entry[0];
            int m2 = entry[1];
            int expected = entry[2];
            return dynamicTest(m1 + " * " + m2 + " = " + expected, () -> {
                assertEquals(expected, tester.multiply(m1, m2));
            });
        });
    }

    // class to be tested
    class MyClass {
        public int multiply(int i, int j) {
            return i * j;
        }
    }
}
```

## 参数化测试（parameterized test）

如果待测试的输入和输出是一组数据，可以使用参数化测试。

使用 `@ParameterizedTest` 注解来标识参数化测试方法，提供的数据需要是静态的：
```java
import static org.junit.jupiter.api.Assertions.*;
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.MethodSource;

public class UsingParameterizedTest {

    public static int[][] data() {
        return new int[][] { { 1 , 2, 2 }, { 5, 3, 15 }, { 121, 4, 484 } };
    }

    @ParameterizedTest
    @MethodSource(value =  "data")
    void testWithStringParameter(int[] data) {
        MyClass tester = new MyClass();
        int m1 = data[0];
        int m2 = data[1];
        int expected = data[2];
        assertEquals(expected, tester.multiply(m1, m2));
    }

    // class to be tested
    class MyClass {
        public int multiply(int i, int j) {
            return i * j;
        }
    }
}
```

### 参数提供

参数化测试有多种提供测试参数的方法：

@ValueSource 注解：
```java
@ValueSource(ints = { 1, 2, 3 })
```
参数以数组方式传入，支持 `String` ，`int`，`long`，`double` 类型。

@EnumSource 注解：
```java
@EnumSource(value = Months.class, names = {"JANUARY", "FEBRUARY"})
```
传入枚举类

@MethodSource 注解：
```java
@MethodSource(names = "genTestData")
```
传入静态方法

@CsvSource 注解：
```java
@CsvSource({ "foo, 1", "'baz, qux', 3" })
```
传入 CSV 格式

@ArgumentSource 注解：
```java
@ArgumentsSource(MyArgumentsProvider.class)
```
使用类的方式提供数据，该类需实现 `ArgumentsProvider` 接口。

