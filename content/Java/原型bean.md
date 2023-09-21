# 原型bean

spring概念，为5种bean的scope中的一种：prototype。

bean默认为singleton类型，即[[单例模式]]，原型bean的scope为prototype，即每次请求都会创建一个新的bean实例。

当bean为有状态，即保存了用户信息时，默认的单例bean在有状态情况下会出现线程安全问题，这时候就需要使用原型bean。
