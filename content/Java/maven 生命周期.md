# maven 生命周期

maven 包含三套独立的生命周期：

- clean：用于清理项目
- default：用于构建项目
- site：用于构建项目站点

其中，每个生命周期都包含多个阶段（phase）。其中，每个阶段都是有序的，且后面的阶段都依赖于之前的阶段。 ^bdab6a

## clean

clean生命周期的目的是清理项目，它包含3个阶段：

- pre-clean：执行一些clean前需要完成的工作
- clean：清理上一次构建生成的文件
- post-clean：执行一些clean后需要完成的工作

## default

default生命周期的目的是构建项目，定义了真正构建时需要的所有步骤，共包含23个阶段，其中几个重要的阶段如下：

- compile：编译项目的源代码
- test：使用合适的单元测试框架运行测试 , 测试代码不会被打包或部署
- package：将编译后的代码打包成可分发的格式，比如 JAR
- verify：运行任何检查以验证包是否有效并符合质量标准
- install：安装项目包到 maven 本地仓库，供本地其他 maven 项目使用
- deploy：将最终包复制到远程仓库，供其他开发人员和 maven 项目使用

## site

site 生命周期的目的是建立和发布项目站点。  Maven 能够基于`pom.xml`所包含的信息，自动生成一个友好的站点，方便团队交流和发布项目信息。site 周期共包含4个阶段：

- pre-site：执行一些在生成项目站点之前需要完成的工作
- site：生成项目站点文档
- post-site：执行一些在生成项目站点之后需要完成的工作
- site-deploy：将生成的项目站点发布到服务器上

## maven 命令

maven 生命周期命令会执行该阶段前的所有操作，例如`mvn clean`会执行`pre-clean`与`clean`两个阶段。

同时maven也有复合命令，例如`mvn clean install`，指的是在 clean 周期中执行到 `clean` 阶段，然后在 `default` 周期中执行到 `intall` 阶段。


