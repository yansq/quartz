
ThreadLocal主要用来为当前线程存储数据，这个数据只有当前线程可以访问。
可以将ThreadLocal看成一个map，当前线程就是map中的key。ThreadLocal 的 map 采用**开放寻址法**解决哈希冲突。

## 使用方式

```java
ThreadLocal<Integer> threadLocalValue = new ThreadLocal<>();
threadLocalValue.set(1);
Integer result = threadLocalValue.get();
threadLocalValue.remove();
```

`initialValue()`：protected 方法，一般是用来在使用时进行重写的，它是一个延迟加载方法。ThreadLocal 没有被当前线程赋值时，或当前线程刚调用`remove`方法后调用`get`方法，返回此方法值：

```java
public static final ThreadLocal<DateFormat> threadLocal = new
        ThreadLocal<DateFormat>(){ 
    @Override 
    protected DateFormat initialValue() { 
        return new SimpleDateFormat("yyyy-MM-dd"); 
    } 
};
```

在Java8中，ThreadLocal 也提供了静态方法`withInitial`来初始化：

```java
public static <S> ThreadLocal<S> withInitial(Supplier<? extends S> supplier) {
    return new SuppliedThreadLocal<>(supplier);
}
```

传入的参数是一个[[函数式接口#^79694b|Supplier接口]]，通过 Supplier 的`get()`方法获取到初始值:

```java
ThreadLocal<Integer> threadLocal = ThreadLocal.withInitial(() -> 1);
int num = threadLocal.get();
```


## 使用场景

在 ThreadLocal 中存储用户数据：

```java
public class ThreadLocalWithUserContext implements Runnable {

    private static ThreadLocal<Context> userContext
            = new ThreadLocal<>();
    private Integer userId;
    private UserRepository userRepository = new UserRepository();

    public ThreadLocalWithUserContext(int i) {
        this.userId = i;
    }

    @Override
    public void run() {
        String userName = userRepository.getUserNameForUserId(userId);
        userContext.set(new Context(userName));
        System.out.println("thread context for given userId: "
                + userId + " is: " + userContext.get());
    }

}
```

测试代码：

```java
public class ThreadLocalWithUserContextTest {

    @Test
    public void testWithThreadLocal(){
        ThreadLocalWithUserContext firstUser
                = new ThreadLocalWithUserContext(1);
        ThreadLocalWithUserContext secondUser
                = new ThreadLocalWithUserContext(2);
        new Thread(firstUser).start();
        new Thread(secondUser).start();
    }
}
```

测试结果：

```java
thread context for given userId: 1 is: com.flydean.Context@411734d4
thread context for given userId: 2 is: com.flydean.Context@1e9b6cc
```

此外，ThreadLocal主要用于Session管理和数据库链接管理。

## 注意事项

必须回收自定义的 ThreadLocal 变量，尤其在线程池场景下，线程经常会被复用（Tomcat 等 servlet 容器也会复用处理 http 请求的线程）， 如果不清理自定义的 ThreadLocal 变量，可能会影响后续业务逻辑和造成内存泄露等问题。 

尽量在代码中使用 try-finally 块进行回收：

```java
ThreadLocal<Integer> threadLocalValue = new ThreadLocal<>();
threadLocalValue.set(1);
try {
    Integer result = threadLocalValue.get();
} finally {
    threadLocalValue.remove();
}
```