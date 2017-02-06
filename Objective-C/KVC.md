# 键值编程

Objective-C键值编程特性统称为键值编码（Key-Value Coding, KVC）和键值观察（Key-Value Observing, KVO）。使用键值编码可以通过名称（键）间接访问和操作对象的属性，而无须使用访问方法或者支持实例变量。通过键值观察能够使对象在其他对象的属性发生更改时获得通知。

键值编码的设计基于以下两种原则：

* 通过名称（键）间接访问对象的属性，而不是直接调用访问方法；
* 将键和属性访问方法或属性支持变量对应起来；

NSObject类实现了NSKeyValueCoding协议，该协议定义了通过键间接访问对象的机制。该协议声明了键值编码API，含有类、实例方法和常量值。使用这些方法可以获取和设置属性值，执行基于键值编码的属性验证，以及更改用于获取属性键值编码方法的默认行为。

## 使用键值编码获取和设置属性的值

```objective-c
    [NSObject valueForKey:]; // 获取属性的值
    [NSObject setValue:forKey:] // 设置属性的值
```

## 键值编码的优点

与标准的属性访问方法相比，KVC有什么好处？首先通过键值编码可以在运行时使用可变的字符串访问属性，从而能够更加灵活地访问和操作对象的状态。

* 基于配置的属性访问。通过KVC可以使用由参数驱动的通用API访问属性；
* 降低耦合性。通过KVC访问属性可以减少各个软件组件之间的耦合性，从而提高软件的可维护性。
* 简化代码。通过KVC可以减少代码量，在需要根据变量访问指定属性时尤为如此。无须使用条件表达式判断需要调用哪个属性的访问方法，而直接使用KVC表达式，并将变量作为其参数。

## 键和键路径

键值编码使用键和路径访问属性。键用于标识属性的字符串。键路径指明了需要遍历的对象属性序列。KVC API支持访问对象中单个和多个属性，这些属性可以是对象类型、基本类型或C语言结构，基本类型和C语言结构会被自动封装为相应的对象类型，对象类型也可以自动展开为基础类型C语言结构。

```objective-c
    // 通过路径设置属性的值
    [NSObject setValue:forKeyPath:];
```

## 键值编码API

```objective-c
// 使用KVC获取多个属性的值
[NSObject dictionaryWithValuesForKeys:];

// 使用KVC设置多个属性的值
[NSObject setValuesForKeysWithDictionary:];

// 如果没有找到属性的访问方法，键值编码机制是否能直接访问属性的支持变量。YES表示能直接访问实例变量，NO表示不能。NSObject类默认实现会返回YES，通常应该重写这个类，以便控制这种行为。
[NSObject accessInstanceVariablesDirectly];

// 检验属性值
[NSObject validateValue:forKey:error:];
[validateValue:forKeyPath:error];
```

因为键值编码使用键值访问属性，必然可能存在处理的输入键不能与对象的属性对应的情况。使用NSKeyValueCoding协议的valueForUndefinedKey:和setValue:forUndefinedKey:方法可以处理键值编码未定义键的情况。未定义键方法的默认实现会抛出NSUndefinedKeyException。可以在子类中重写这些方法，为未定义的键返回自定义的值。

## KVC是怎样通过键/键路径获取和设置属性值的？—— 键值搜索模式

1. KVC通过搜索接受者类寻找名称符合格式set&lt;key&gt;:的访问方法。其中&lt;key&gt;是属性的名称。
2. 如果没有找到访问方法，接收对象类方法accessInstanceVariablesDirectly就会返回YES，setValue:forKey方法就会搜索对象类，寻找匹配\__key,\_is&lt;key&gt;,key或is&lt;key&gt;格式的实例变量。_
3. 如果找到了匹配访问方法或实例变量，接收对象的setValue:forUndefinedKey:就会被调用。
4. 如果没有找到合适的访问方法和实例变量，接收对象的setValue:forUndefinedKey:就会被调用。

## 属性访问方法的命名约定

* 属性获取方法的名称应该与属性名相同。但是对于布尔类型要在名称前面加is。
* 属性设置方法的名称应使用set&lt;key&gt;格式，就是在属性名前面加set



