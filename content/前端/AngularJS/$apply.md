# $apply


执行`$scope.$apply()`会调用`$rootScope.$digest()`。[[$digest]]会从根作用域开始遍历，调用各个作用域中的[[$watch]]。

angularJS几乎在自己的所有API中都添加了`$apply`，只有当更改一个不在angularJS执行上下问的model时，才需要动手进行`$apply`，一个典型的场景就是在`setTimeout()`函数中，不过angularJS也提供了对应的`$timout`api来进行自动`$apply`。

在手动调用`$apply()`的时候，建议添加参数，不然会检查[[$scope]]中的所有[[$watch]]；并且`$aply()`中存在try/catch机制，而[[$digest]]运行在finally语句中，因此当传入参数时，无论是否有错误，程序都会正常执行。

