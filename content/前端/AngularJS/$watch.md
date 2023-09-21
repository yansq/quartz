# $watch

当一个作用域创建的时候，angular会去解析模板中当前作用域下的模板结构，并且自动将那些[[插值]]（如`{{text}}`）或调用（如ng-click="update"）找出来，并利用`$watch`建立绑定，它的回掉函数用于决定如果新值和旧值不同时要执行的操作。

使用`$watch`手动监听属性的语法为
```js
$scope.$watch(string|function, listener, objectEquality, prettyPritExpression)
```
- string|function：第一个参数是一个字符串或函数。如果是函数的话，需要函数的返回值为字符串。这个参数用于确定将监听[[$scope]]的哪个属性。
- listener：回调函数。
- objectEquality：boolean类型，为true时，会对object进行深检查（默认浅检查）。
- prettyPritExpression：如何解析第一个参数的表达式，一般用不到。


