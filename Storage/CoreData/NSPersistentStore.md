# NSPesistentStore
是Core Data持久存储的基类。

## 概述
Core Data提供了四种持久存储类型——SQLite, Binary, XML, and In-Memory (XML存储在iOS中不可用);还提供了 NSAtomicStore and NSIncrementalStore两个类以便用户自定义的持久存储类型。Binary和XML存储是原子存储的示例，从NSAtomicStore继承了此功能。

子类化时不能直接继承NSPersistent.Core Data支持NSAtomicStore和NSIncrementalStore的子类化。
