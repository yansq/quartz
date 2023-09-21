# maven dependencyManagement

用法示例：

```xml
<dependencyManagement> 
    <dependencies> 
        <dependency> 
            <groupId>org.apache.commons</groupId> 
            <artifactId>commons-lang3</artifactId> 
            <version>3.12.0</version> 
        </dependency> 
    </dependencies> 
</dependencyManagement>
```

使用 dependencyManagement 可以统一管理项目的版本号，确保应用的各个项目的依赖和版本一致，利于管理。当需要变更版本号时，只需要在父类容器里更新即可；如果某个子项目需要另外一个特殊的版本号时，只需要在自己的模块dependencies中声明一个版本号即可。子类就会使用子类声明的版本号，不继承于父类版本号。

## 与 dependency 的区别

1. Dependencies 相对于 dependencyManagement，所有声明在 dependencies里的依赖都会自动引入，并默认被所有的子项目继承。
2. dependencyManagement 里只是声明依赖，并不自动实现引入，因此子项目需要显式声明需要的依赖。如果不在子项目中声明依赖，是不会从父项目中继承下来的；只有在子项目中写了该依赖项，并没有指定具体版本，才会从父项目中继承该项，并且 version 和 scope 都读取自父 pom;另外如果子项目中指定了版本号，那么会使用子项目中指定的 jar 版本。


## 常见错误

在 dependencyManagement 中定义依赖关系,而不在 dependency 中引用。.在这种情况下，将遇到编译或运行时错误。.