# XSD(XML Schemas Definition)

XSD，即XML Schema，描述了XML文档的结构。 可以用一个指定的XML Schema来验证某个XML文档，以检查该XML文档是否符合其要求。XML Schema本身是一个XML文档，它**符合XML语法结构**，可以用通用的XML解析器解析。 

XML Schema会定义：
- 文档中出现的元素/属性；
- 子元素、子元素的数量、子元素的顺序；
- 元素是否为空、元素和属性的数据类型、元素或属性的默认值和固定值。

XSD是[[DTD]]替代者，因为：
1. 可根据将来的条件可扩展；
2. 比[[DTD]]丰富和有用；
3. 用XML书写；
4. 支持数据类型；
5. 支持命名空间。

使用方式：
```xml
<beans xmlns="http://www.springframework.org/schema/beans"  
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"   
    xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans-2.5.xsd>  
```