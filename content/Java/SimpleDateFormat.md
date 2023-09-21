# SimpleDateFormat

SimpleDateFormat是一个格式化日期与解析日期的工具类，允许自定义日期格式。

## 使用方式

```java
// Date 转 String
Date date = new Date();
SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
String dateStr = sdf.format(data);
System.out.prinln(dataStr);


// String 转 Date
System.out.println(sdf.parse(dateStr));
```

## 非线程安全

SimpleDateFormat的format方法在执行过程中，会使用一个成员变量calendar来保存时间，导致线程不安全

阿里巴巴Java开发手册中，有以下规定：
> \[强制\] SimpleDateFormat 是线程不安全的类，一般不要定义为 static 变量，如果定义为 static，必须加锁，或者使用 DateUtils 工具类。

在开发手册中推荐使用[[ThreadLocal]]来避免多线程竞争：
```java
private static final ThreadLocal<DateFormat> df = new ThreadLocal<DateFormat>() {
    @Override
    protected DateFormat initialValue() {
        return new SimpleDateFormat("yyyy-MM-dd");
    }
};
```
也可以：
```java
private static final ThreadLocal<DateFormat> df = new ThreadLocal.withInitial(() -> new DateFormat("yyyy-MM-dd"));
```