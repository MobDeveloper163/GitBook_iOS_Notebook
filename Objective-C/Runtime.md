#Runtime运行时系统

## 动态绑定
动态绑定（dynamic binding）是指在运行程序时(而不是在编译时)将消息和方法对应起来的处理过程。

## 动态方法决议
使用动态方法决议能够以动态方式实现方法。NSObject类中有resolveInstanceMethod:和resolveClassMethod:方法，它们能够以动态方式分别为指定的实例和类方法选择器提供**实现代码**。可以重写这些方法实现实例/类方法。

