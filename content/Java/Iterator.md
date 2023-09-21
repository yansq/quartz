# Iterator

`Iterator`是一种抽象的集合数据访问模型，Java的集合类通常都会实现`java.util.Iterable`接口，使用`Iterator`模式进行迭代的好处有：

- 对任何集合都采用同一种访问模型
- 调用者对集合内部结构一无所知
- 集合类返回的`Iterator`对象知道该如何迭代