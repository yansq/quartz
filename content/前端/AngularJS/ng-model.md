# ng-model

ng-model区别于[[ng-bind]]，为[[双向绑定]]（$scope -> view & view -> scope）。如：`<input type="text" ng-model="val" />`

如果当前作用域上没有ng-model所绑定的属性，将隐式创建该属性，并将其添加到作用域。