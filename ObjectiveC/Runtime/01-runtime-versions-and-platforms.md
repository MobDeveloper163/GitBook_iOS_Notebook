# Legacy & Modern Version Runtime

## 在不同的平台上有不同版本的runtime

iPhone应用程序，OS X V10.5及以后版本上的64位程序使用modern version.

OS X上的32位程序使用lagacy version.

## Modern 和 Legacy版本的区别

最值得关注的特性是实例变量在moder runtime中“ non-fragile ”

* 在legacy runtime中，如果你修改了类中的实例变量的layout，你必须重新编译集成这个类的的所有的类。
* 在modern runtime中，如果你修改了类中的实例变量的layout，你不需要重新编译集成这个类的的所有的。

另外，modern runtime支持声明属性的实例变量合成。

