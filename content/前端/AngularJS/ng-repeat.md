# ng-repeat

```html
<h1 ng-repeat="x in records">{{x}}</h1>
```

## track by

ng-repeat的数组中如果存在相同项时，会报错。可以通过添加track by的方式避免报错：
```html
<h1 ng-repeat="x in records track by $index">{{x}}</h1>
```

当添加`track by`后，当循环渲染的数据被更改时，不会删除所有DOM重新生成（默认情况），而是在现有DOM基础上进行修改。因此在ng-repeat中添加`track by`后，可以一定程度上提升性能。

