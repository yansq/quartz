# maven 插件

maven 的[[maven 生命周期|生命周期]]定义了多个阶段，其中每个阶段要实现的具体功能都是由**插件**来完成。

一个**插件** 包含多个**插件目标**（Plugin Goal），每个**插件目标**可以实现一项特定的功能。

maven生命周期的[[maven 生命周期#^bdab6a|阶段]]与**插件目标**绑定，以完成某个具体的构件任务。例如项目编译这个任务，对应了`default生命周期`阶段的`compile阶段`，而`maven-compiler-plugin`插件的`compile目标`能够完成该任务，因此将他们进行绑定，以实现项目编译任务。

为了让用户几乎不用任何配置就能构建 maven 项目，maven 为一些主要的生命周期阶段绑定好了插件目标，当我们通过命令调用生命周期阶段时，绑定的插件目标就会执行对应的任务。

## 自定义绑定

用户可以自己选择将某个插件目标绑定到生命周期的某个阶段上，使得 maven 项目在构建过程中执行个性化任务。

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-source-plugin</artifactId>
            <version>3.2.1</version>
            <executions>
                <execution>
                    <id>attach-sources</id>
                    <phase>verify</phase>
                    <goals>
                        <goal>
                            jar-no-fork
                        </goal>
                    </goals>
                </execution>
            </executions>

        </plugin>
    </plugins>
</build>
```

将`maven-source-plugin`插件的`jar-no-fork`目标，绑定到 [[maven 生命周期#default | default]] 
生命周期的`verify`阶段。

## 参考资料

[Maven学习5: 生命周期和插件](https://segmentfault.com/a/1190000021364596)
