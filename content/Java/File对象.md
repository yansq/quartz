# File对象

Java标准库`java.io`提供了`File`对象来操作文件和目录。

要构造一个`File`对象，需要传入文件路径：
```java
File f = new File("C:\\Windows\\notepad.exe");
```
注意，Windows平台使用`\`作为路径分隔符，**在Java字符串中需要使用`\\`来表示一个`\`**。

`File`对象既可以表示文件，也可以表示目录。要注意的是，构造一个`File`对象，即使传入的文件或目录不存在，代码也不会报错。因为构造一个`File`对象，并不会导致任何磁盘操作。只有调用`File`对象的某些方法的时候，才真正进行磁盘操作。

使用`isFile()`判断`File`对象是否是一种已存在的文件；使用`isDirectory()`判断是否是一个已存在的目录。

判断文件的权限和大小：
- `boolean canRead()`
- `boolean canWrite()`
- `boolena canExecute()`
- `long length()`：文件字节大小
