# AOP(面向切面编程)

将一些重复的、和业务主逻辑不相关的功能性代码（日志记录、安全管理等）通过切面模块化地抽离出来进行封装，实现关注点分离、模块解耦，使得整个系统更易于维护管理。

## 名词解释

- `Aspect`： 切面是`Pointcut`和`Advice`的集合，一般单独作为一个类。`Pointcut`和`Advice`共同定义了关于切面的全部内容：在**在何时和何处**完成**何功能**。
-  `Joinpoint`： 表示应用程序中可以插入 AOP 方面的一点。也可以说，是应用程序中使用 Spring AOP 框架采取操作的实际位置。 
-  `Advice` ：在方法执行之前或之后采取的实际操作，是在 Spring AOP 框架的程序执行期间调用的实际代码片段。 
-  `Pointcut` ：切点，这是一组一个或多个切入点，在切点应该执行`Advice`。 可以使用表达式或模式指定切入点.
-  `Introduction` ：允许向现有的类添加新的方法或者属性 
-  `Weaving`： 创建一个被增强对象的过程。这可以在编译时完成（例如使用AspectJ 编译器），也可以在运行时完成。Spring 和其他纯 Java AOP 框架一样，在运行时完成织入。

## Demo

### 开启 @Aspect 注解

有两种方式：
1. 在XML中配置：`<aop:aspectj-autoproxy/>`
2. 使用`@EnableAspectJAutoProxy`注解：
```java
@Configuration
@EnableAspectJAutoProxy
public class Config {

}
```

### 编写目标类

```java
package ric.study.demo.aop.svc;

public interface TestSvc {
    void process();
}

@Service("testSvc")
public class TestSvcImpl implements TestSvc {
    @Override
    public void process() {
        System.out.println("test svc is working");
    }
}

public interface DateSvc {
    void printDate(Date date);
}

@Service("dateSvc")
public class DateSvcImpl implements DateSvc {
    @Override
    public void printDate(Date date) {
        System.out.println(new SimpleDateFormat("yyyy-MM-dd HH:mm:ss").format(date));
    }
}

```


### 配置Pointcut

```java
@Aspect 
@Component 
public class PointCutConfig { 
    @Pointcut("within(ric.study.demo.aop.svc..\*)") 
    public void inSvcLayer() {} 
}

```

@Aspect：被`@AspectJ`注解的 **bean(必须有对应的bean注解，如`@Component`)** 都会被 Spring 当做是 AOP 配置类，称为一个 Aspect。
@Pointcust：匹配 Spring 容器中所有满足指定条件的 bean 的方法，匹配方式：

1. execution：匹配方法签名
```java
@Pointcut("execution(* testExecution(..))") 
```
匹配名为`testExecution`方法，`*`表示任意返回值，`(..)`表示零个或多个参数 

2. within：指定所在类或所在包下面的方法（Spring AOP 独有）
```java
@Pointcut("within(ric.study.demo.aop.svc..*)")
```
`..`表示包和子包


3. @annotation：方法上有特定的注解
```java
@Pointcut("@annotation(ric.study.demo.aop.HaveAop)")
```

4. bean：匹配bean的名字（Spring AOP独有）
```java
@Pointcut("bean(testController)")
```

### 配置Advice

```java
@Component 
@Aspect 
public class ServiceLogAspect { 
    // 拦截，打印日志，并且通过JoinPoint 获取方法参数
    @Before("ric.study.demo.aop.PointCutConfig.inSvcLayer()")
    public void logBeforeSvc(JoinPoint joinPoint) {
        System.out.println("在service 方法执行前 打印第 1 次日志");
        System.out.println("拦截的service 方法的方法签名: " 
            + joinPoint.getSignature()); 
        System.out.println("拦截的service 方法的方法入参: " 
            + Arrays.toString(joinPoint.getArgs())); 
    }
    
    // 这里是Advice和Pointcut 合在一起配置的方式
    @Before("within(ric.study.demo.aop.svc..\*)") 
    public void logBeforeSvc2() { 
        System.out.println("在service的方法执行前 打印第 2 次日志"); 
    } 
}
```

## 原理

Spring提供了两种方式来生成代理对象: [[JDKProxy]]和[[Cglib]]，具体使用哪种方式生成由 `AopProxyFactory` 根据 `AdvisedSupport` 对象的配置来决定。默认的策略是如果目标类是接口，则使用JDK动态代理技术，否则使用Cglib来生成代理。

https://zhuanlan.zhihu.com/p/36617574