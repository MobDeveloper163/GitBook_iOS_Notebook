## NSThread

NSThread提供了用于显式创建和管理线程的API，该类包含的方法可以创建和初始化NSThread对象、启动和停止线程、配置线程和查询线程及其执行环境。

创建和传输化线程的API：

```objective-c
// 方式一：创建并启动线程
detachNewThreadSelector:toTarget:withObject:
// 方式二：只创建线程，不启动线程
initWithTarget:selector:object
```

暂停线程：
```objective-c
[NSThread sleepForTimeInterval:5.0];
```






