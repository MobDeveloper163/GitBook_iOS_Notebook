# 键值编程
Objective-C键值编程特性统称为键值编码（Key-Value Coding, KVC）和键值观察（Key-Value Observing, KVO）。使用键值编码可以通过名称（键）间接访问和操作对象的属性，而无须使用访问方法或者支持实例变量。通过键值观察能够使对象在其他对象的属性发生更改时获得通知。

## 使用键值编码获取和设置属性的值

```objective-c
    [NSObject valueForKey:]; // 获取属性的值
    [NSObject setValue:forKey:] // 设置属性的值
```

## 键值编码的优点

