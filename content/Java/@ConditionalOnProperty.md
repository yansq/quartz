# @ConditionalOnProperty

`@ConditionalOnProperty`可以用来控制配置类是否生效。

```java
@Configuration
@ConditionalOnProperty(value = "ipub.cache.redis.enabled", matchIfMissing = true)
class Redis Config {

}
```

