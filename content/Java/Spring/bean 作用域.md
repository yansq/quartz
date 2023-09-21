# bean 作用域

#spring 

bean有五种作用域，默认为单例：

1. `Singleton`（单例类型），容器启动时就实例化和初始化；
2. `Prototype`（原型类型），容器启动时没有实例化，只有获取 bean 时才被创建，每次都新建一个实例；
3. `request`（web 的 Spring AllicationContext 下），每个 HTTP 都有自己的 bean，当处理结束时销毁；
4. `session`（web 的 Spring ApplicationContext 下），每个 HTTP session 都有自己的 bean；
5. `global session`（web 的 Spring ApplicationContext 下）。

使用单例bean需要关注多线程安全，如果需要保存状态，要么使用 `Prototype` 类型，加上`@scope("prototype")`；要么把需要的可变成员变量保存在 [[ThreadLocal]]中。
