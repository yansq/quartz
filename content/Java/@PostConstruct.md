# @PostConstruct

用于修饰*非静态的 void 方法*，被修饰的方法会在 Servlet 加载时执行一次，且只执行一次。

执行顺序如下：

```text
构造器
    --> 自动注入(@Autowired)
        --> @PostConstrut
            --> InitializingBean
                --> xml中配置init方法
```


使用场景：

有些初始化动作需要用到自动注入的 bean，如果在构造器中写初始化逻辑，由于执行顺序原因，执行到构造器时 bean 还没有被注入。这种情况下，可以将初始化逻辑放在`@PostConstrut`中执行。

如为工具类读取配置文件中的配置：
```java
/**  
 * 静态方法读取配置用  
 */
@Configuration  
public class ConfigurationReader { 

    public static long TERMINAL_ID;  
  
    public static long DATACENTER_ID;  
  
    @Value("${terminal.id:1}")  
    private long terminalId;  
  
    @Value("${datacenter.id:1}")  
    private long datacenterId;  
  
    @PostConstruct  
    public void setConfigs() {  
        TERMINAL_ID = this.terminalId;  
        DATACENTER_ID = this.datacenterId;  
    }
}
```