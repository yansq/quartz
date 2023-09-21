# @Qualifier注解

Spring注解。在使用`@Autowired`进行依赖注入时，Spring会先去寻找`@Primary`注解，其次是`@Priority`注解，最后按bean的名称来实现注入。当出现以下情形时：
```java
    @Component("fooFormatter")
    public class FooFormatter implements Formatter {
        public String format() {
            return "foo";
        }
    }

    @Component("barFormatter")
    public class BarFormatter implements Formatter {
        public String format() {
            return "bar";
        }
    }

    @Component
    public class FooService {
        @Autowired
        private Formatter formatter;
        
        //todo 
    }
```
同时有两个实现Formatter接口的bean存在，又没有一个bean的名称为`Formatter`，spring在执行时会报 `NoUniqueBeanDefinitionException`异常，使用`@Qualifier`注解是解决这一问题的方法之一。

通过使用`@Qualifier`注解，可以指定特定的Spring bean名称，避免spring[[脑裂]]:
```java
   @Component
    public class FooService {
        @Autowired
        @Qualifier("fooFormatter")
        private Formatter formatter;
        
        //todo 
    }

```


需要注意的是，如果显示制定bean的名称，spring会自动生成bean的名字，**如果一个类名是以两个及以上大写字母开头的，首字母保持大写；其他情况下首字母为小写**，在使用`@ualifier`注解制定名称的时候需要注意。

