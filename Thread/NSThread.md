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

设置线程优先权：
```objective-c
[NSThread setThreadPriority:0.5];
```

## 线程同步
1. 锁
    Foundation矿建含有多个类（NSLock、 NSRecursiveLock、NSConditionLock和NSDistributedLock），使用这些类可以实现各种用于同步访问贡状态操作的锁。锁用于保护关键部分（即用于访问共享数据或资源的共享部分），这些代码不允许由多个线程以并发方式执行。
    NSLock类为并发编程提供了一种基本的互斥锁。它遵循NSLocking协议，并因此会实现分别用于获取和释放锁的lock和unlock方法。







