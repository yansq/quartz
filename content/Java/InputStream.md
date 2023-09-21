# InputStream

读取[[IO]]流。

`InputStream`使用[[装饰器模式]]来添加额外的功能。

JDK将`InputStream`分为两大类
一类是直接提供数据的基础`InputStream`，例如：
- FileInputStream
- ByteArrayInputStream
- ServletInputStream
- ...

另一类是提供额外附加功能，例如：
- BufferedInputStream
- DigestInputStream
- ClipherInputStream

举个例子，先为`InputStream`添加从文件读取的功能：
```java
InputStream file = new FileInputStream("test.gz");
```
再添加缓冲功能来提供读取效率：
```java
InputStream buffered = new BufferedInputStream(file);
```
再添加读取压缩文件的功能：
```java
InputStream gzip = new GZIPInputStream(buffered);
```
以上对`InputStream`进行了若干次包装，但每一次得到的对象仍然是`InputStream`。这样设计是为了避免排列组合，导致`InputStream`的子类数量爆炸的问题。

`OutputStream`也是以这种模式来提供各种功能。

## 使用方式

`read()`方法返回字节表示的值（0~255）
```java
public void readFile() throws IOException {
    InputStream input = null;
    try {
        input = new FileInputStream("src/readme.txt");
        int n;
        while ((n = input.read()) != -1) { // 利用while同时读取并判断
            System.out.println(n);
        }
    } finally {
        if (input != null) { input.close(); }
    }
}
```

利用缓冲区提高读取效率：
先定义一个`byte[]`数组作为缓冲区，`read()`方法会尽可能多地读取字节到缓冲区， 但不会超过缓冲区的大小。`read()`方法的返回值不再是字节的`int`值，而是返回实际读取的字节数。如果返回`-1`，表示没有更多的数据。
```java
public void readFile() throws IOException {
    try(InputStream input = new FileInputStream("src/readme.txt")) {
        // 定义1024个字节大小的缓冲区:
        byte[] buffer = new byte[1024];
        int n;
        while((n = input.read(buffer)) != -1) { // 读取到缓冲区
            System.out.println("read " + n + " bytes.");
        }
    }
}
```