Spring注解，当一个默认的单例bean引用一个[[原型bean]]的时候。在创建该单例bean的时候，会使用构造器[[反射]]出实例，然后装配加了@Autowired注解的[[原型bean]]，这一过程只会执行一次，从而导致[[原型bean]]失去了多例属性。

添加一个get方法，返回值为该[[原型bean]]的类型，并对该方法使用`@Lookup`注解可以解决此问题:
```java
@Lookup 
public ServiceImpl getServiceImpl(){ 
    return null; 
}
```

当使用`@Lookup`注解后，spring会通过BeansFactory获取bean，这个方法会被重写，`getServiceImpl`方法中的任何实现都不会执行。

