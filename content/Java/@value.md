# @value

Spring概念，用于属性的注入。

```java
//注册正常字符串
@Value("我是字符串")
private String text; 

//注入系统参数、环境变量或者配置文件中的值
@Value("${ip}")
private String ip

//注入其他Bean属性，其中student为bean的ID，name为其属性
@Value("#{student.name}")
private String name;
```

@value除最为常见的对String对象进行装配外，还可用于非内置对象：
```java
@Value("#{student}")
private Student student;


@Bean
public Student student(){
    Student student = createStudent(1, "xie");
    return student;
}
```


