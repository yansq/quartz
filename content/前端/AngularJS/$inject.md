# $inject
$inject是AngularJS提供的依赖注入方法。

一个通常情况下的Controller:
```javascript
var Ctrl = function($scope, notify) {
    $scope.name = 'Tom' 
}
```

如果对前端代码进行最小化或[[丑化]]，上面代码中的[[$scope]]可能会被转化为类似于`a`或`b`的变量，而这些变量无法被AngularJS识别，导致报错。

AngularJS提供了`$inject`方法定义上述变量：
```javascript
var Ctrl = function(a, b) {
    a.name = 'Tom' 
}

// content

Ctrl.$inject = ['$scope', 'notify'];
```
数组中变量的位置与Controller中参数的位置一一对应，AngularJS会寻找`$inject`中注入的值，将之绑定到Controller中。如存在上述代码，在Crontroller中可以使用`a`来代替`$scope`，`b`来代替`notify`。

不过因为代码的可读性考虑，一般不建议使用上述写法。使用`$inject`的主要目的就是为了在代码压缩过后，保证程序的正常执行，所以一般采用相同的参数进行绑定：
```javascript
var Ctrl = function($scope, notify) {
    a.name = 'Tom' 
}

// content

Ctrl.$inject = ['$scope', 'notify'];
```

如果在使用[[Module]]的时候需要使用`$inject`，可以采用以下写法：
```javascript
var myApp = angular.module('MyApp', []);
 
myApp.controller('Ctrl', ['$scope', function($scope){
	$scope.name = 'Tom';
}]);
```