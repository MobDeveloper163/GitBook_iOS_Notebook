## Objective-C在三个不同的层次上与runtime系统进行交互。三个层次是：

* 通过Objective-C源代码
* 通过Foundation框架的NSObject类中定义的方法
* 直接调用runtime方法

### Objective-C源代码

大部分，运行时系统自动在后台工作。使用它，你仅仅需要写和编译Objective-C源代码。


当你编译的代码包含OC类和方法时，编译器创建数据结构和实现了动态特性的方法调用。数据结构捕获在类、分类定义和协议声明中找到的信息runtime主要的功能之一发送消息。

### NSObjective-C方法

Cocoa中大部分对象都是NSObject的子类，因此大部分对象都集成NSObject的定义的方法（需要注意的例外是NSProxy类。查看消息转发（Message Forwarding）查看更多信息）。因此它的方法确定了继承它的每一个实例和类对象的行为。但是，在很少的情况中，NSObject类仅仅定义一些事情应该怎样完成的模板，它自己并不提供所有必须的源代码。



比如,NSObject类定义了description实例方法，返回一个字符串描述类的内容。主要用来debugging的.NSObject的这个方法的实现并不知道类到底包含了什么，所以它返回这个对象的名字和地址的字符串。NSObject的子类可以实现这个方法，返回更详细的信息。



有一些对象方法简单地向runtime系统查询信息。这些方法允许对象自省。比如这些方法：class方法，请求对象指认自己的类；isKindOfCass:和isMemberOfClass,测试在继承层级中，一个对象的位置；responseToSelector:，表明一个对象能否接收特定的消息；conformToProtocol:,表明一个对象是否实现了一个特定协议定义的方法；methodForSelector:,提供一个方法实现的地址。像这样的方法给对象反省自己的能力。



### 运行时方法

运行时系统是一个在 \/usr\/include\/objc 文件夹下的头文件中，由一组函数和数据结构组成的公共接口的动态共享库。当你写OC代码，其中许多的方法允许使用纯C来复制编译器做的事情。其它的组成了通过NSObject导出的方法的基础。这些方法使得开发运行时的其他接口和增强开发环境的生产工具成为可能，在使用OC编程的时候并不需要他们。然而有时在写OC程序时，一些运行时方法会很有用。所有这些方法记录在 [Objective-C Runtime Reference](https://developer.apple.com/reference/objectivec/1657527-objective_c_runtime). 









