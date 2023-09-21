# $digest

`$digest`是angularJS[[脏检查]]机制的核心，[[AngularJS]]在digest循环中遍历[[$watch]]监听器，当model的值发生改变时，就去调用相应[[$watch]]中的回调函数。