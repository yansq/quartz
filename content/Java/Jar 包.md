# Jar 包

Jar package contains：
- Jar description,  `META_INF/MANIFEST.MF`，includes  info likes constructor info and starting class info.
- Spring Boot Loader，`org/springframework/boot/loader`
- project content，`BOOT-INF/classes`
- project dependencies，`BOOT_INF/lib`

**Jar package does't include JDK or JRE**

Jar package running without `java -jar` command：
```xml
<plugin>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-maven-plugin</artifactId>
    <configuration>
        <!-- this configuration -->
        <executable>true</executable>
    </configuration>
</plugin>
```

