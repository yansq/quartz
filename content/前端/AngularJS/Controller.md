# Controller

[[AngularJS]]基于[[MVC]]模式开发，Controller的作用是连接模型层(Model)与视图层(View)。
Controller是绑定在DOM上的，一个View可以对应多个Controller，Controller之间也可以进行嵌套。

Controller的作用域由[[$scope]]传入，[[$scope]]对象只能调用作用域范围内的属性和方法。
在当前作用域中无法找到属性的时候，会去父作用域中查找，类似于JS[[原型链]]的模式。

常规的Controller与View的绑定方式：
```html
<div ng-app="myApp">
    <div ng-controller="MainController">
        <p>Hello {{name}}</p>
    </div>
</div>    
```

```js
var app = angular.module('myApp', []);
app.controller('MainController', ['$scope', function($scope) {
    $scope.name = "world";       
}])
```

使用ControllerAs，可以绕过[[$scope]]，直接创建Controller的实例，将之与View绑定：
```html
<div ng-app="myApp">
    <div ng-controller="MainController as mainCtrl">
        <p>Hello {{mainCtrl.name}}</p>
        <div>{{content}}</div>
    </div>
</div>    
```

```js
var app = angular.module('myApp', []);
app.controller('MainController', function() {
    this.name = "world"; 
    this.content = "aaaa";
})
```

使用ControllerAs语法的优点：
由于可以在View层的属性上自定义Controller名称，作用域更加清晰。