# The bean 'SERVER.FeignClientSpecification', defined in null, could not be registered.

## 完整报错信息

```bash
***************************  
APPLICATION FAILED TO START  
***************************  
Description:  
The bean 'SERVER.FeignClientSpecification', defined in null, could not be registered. A bean with that name has already been defined in null and overriding is disabled.  
  
Action:  
Consider renaming one of the beans or enabling overriding by setting spring.main.allow-bean-definition-overriding=true  
  
Process finished with exit code 1
```

## 报错原因：

存在多个被`@FeignClinet`装饰的 Feign 接口类，且配置的`name`（即服务名）属性相同。

在注册 feign client 的时候，生成了相同名称的`FeignClientSpecification` bean。

## 解决方案

`@FeignClient`注解中存在`contextId`属性，可用于指定服务的别名。

在 Feign 注册类中，如果指定了`contextId`，就会将`contextId`作为 client name。给相同服务的 Feign Client 配置不同的`contextId`，即可解决 bean 名称冲突的问题：
```java
// org.springframework.cloud.openfeign.FeignClientsRegistrar  
class FeignClientsRegistrar implements ImportBeanDefinitionRegistrar, ResourceLoaderAware, EnvironmentAware {  
    private String getClientName(Map<String, Object> client) {  
        if (client == null) { return null; }  
          
        String value = (String) client.get("contextId");  
        if (!StringUtils.hasText(value)) { value = (String) client.get("value"); }  
        if (!StringUtils.hasText(value)) { value = (String) client.get("name"); }  
        if (!StringUtils.hasText(value)) { value = (String) client.get("serviceId"); }  
        if (StringUtils.hasText(value)) { return value; }  
        throw new IllegalStateException("Either 'name' or 'value' must be provided in @" + FeignClient.class.getSimpleName());  
    }  
}
```


## 仍存在报错

实战中发现，在做了`contextId`的区分后，仍存在上述报错。打断点调试后发现，每个 Feign Client 都被注册了2遍。后续查明这一遍多的注册是由于星云框架导致。

解决方案是在保证`contextId`区分的基础上，配置`spring.main.allow-bean-definition-overriding`属性为 true，允许同名 bean 的覆盖。这个配置在 SpringBoot 2.0.x 版本默认为 true，在之后的版本中默认为 false，需要注意。