# Category（分类）

[参考链接：http:\/\/blog.csdn.net\/a316212802\/article\/details\/49894421](/Category作用和内部实现原理)

## Category的主要作用

* 为已经存在的类添加方法 
* 可以把类的实现放在不同的文件

  * 减少单个文件的体积
  * 把不同的功能组织到不同的category里
  * 可以由多个开发者共同完成一个类
  * 可以按需加载想要的category

* 声明私有方法

* 模拟多继承
* 把framework的私有方法公开 

## Category和Extension的区别

Category在运行时决议，category无法添加实例变量（因为在运行时，对象的内存布局已经确定，如果添加实例变量就会改变类的内部布局，这对编译型语言来说是灾难性的）。

它就是类的一部分，在编译期和头文件里的@interface以及实现文件里的@implement一起形成一个完整的类，它伴随类的产生而产生，亦随之一起消亡。extension一般用来隐藏类的私有信息，你必须有一个类的源码才能为一个类添加extension，所以你无法为系统的类比如NSString添加extension。

## Category的本质

所有的OC类和对象，在runtime层都是结构体，Category也一样。Category在runtime中的结构体为category\_t.

```
typedef struct category_t {
     const char *name; // 类的名字
     classref_t cls;  // 类
     struct method_list_t *instanceMethods; // 给类添加的实例方法列表
     struct method_list_t *classMethods;     // 给类添加的类方法列表
     struct protocol_list_t *protocols;     // 分类实现的协议列表
     struct property_list_t *instanceProperties;     // 分类添加的所有的属性
}  category_t;
```

从Category的结构体定义也可以看出Category能做的事情：

> 可以添加实例方法，类方法，甚至可以实现协议，添加属性

和不能做的事情

> 无法添加实例变量

## Category是怎样加载到类中的

Objective-C的运行是依赖OC的runtime的，而OC的runtime和其他系统库一样，是OS X和iOS通过dyld动态加载的。runtime做了以下事情：

1. 把category的实例方法、协议以及属性添加到类上 
2. 把category的类方法和协议添加到类的metaclass上

**需要注意的有两点：**

1. Category的方法没有“完全替换掉”原来类已经有的方法，也就是说如果Category和原来类都有methodA，那么category附加完成之后，类的方法列表里会有两个methodA
2. Category的方法被放到了新方法列表的前面，而原来类的方法被放到了新方法列表的后面，这也就是我们平常所说的Category的方法会“覆盖”掉原来类的同名方法，这是因为运行时在查找方法的时候是顺着方法列表的顺序查找的，它只要一找到对应名字的方法，就会罢休^\_^，殊不知后面可能还有一样名字的方法。

## Category和关联对象

我们知道在category里面是无法为category添加实例变量的。但是我们很多时候需要在category中添加和对象关联的值，这个时候可以求助关联对象来实现。



**关联对象又是存在什么地方呢？ 如何存储？ 对象销毁时候如何处理关联对象呢？**

所有的关联对象都由AssociationsManager管理. AssociationsManager里面是由一个静态AssociationsHashMap来存储所有的关联对象的。这相当于把所有对象的关联对象都存在一个全局map里面。而map的的key是这个对象的指针地址（任意两个不同对象的指针地址一定是不同的），而这个map的value又是另外一个AssociationsHashMap，里面保存了关联对象的kv对。 



untime的销毁对象函数objc\_destructInstance里面会判断这个对象有没有关联对象，如果有，会调用\_object\_remove\_assocations做关联对象的清理工作


