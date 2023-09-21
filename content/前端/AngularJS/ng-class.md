# ng-class

ng-class用于对HTML元素动态绑定类名，ng-class的值可以为：字符串，对象，数组。
```html
<div ng-class=testClass></div>
<div ng-class={testClass:true, redClass:color="red"}></div>
<div ng-class=[testClass,{redClass:color="red"}]></div>
```
在ng-class中使用[[模板字符串]]时似乎会有问题，建议不要使用。