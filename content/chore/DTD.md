# DTD(Document Type Definition)

DTD是一种XML约束模式语言，属于XML文件的一部分。DTD是一种保证XML文档格式正确性的方法，可以通过比较XML文档与DTD文件来查看文档是否符合规范，元素和标签使用是否正确。

一个DTD文档包含：
- 元素的定义规则
- 元素间关系的定义规则
- 元素可使用的属性
- 可使用的实体或符号规则

使用DTD需要在XML文件投产进行声明，如：
```xml
<!DOCTYPE beans PUBLIC "-//SPRING//DTD BEAN //EN" "http://www.springframework.org/dtd/spring-beans.dtd">
```


## see also

另一种XM验证方式[[XSD]]