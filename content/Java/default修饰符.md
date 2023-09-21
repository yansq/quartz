# default修饰符

在Java 8中引入的修饰符，用于修饰接口中的方法。在加了default修饰符后，允许在接口中实现方法，并且接口的实现类不需要实现该方法。

```java
public interface Resource extends InputStreamSource {
    default boolean isOpen() { return false };
}
```