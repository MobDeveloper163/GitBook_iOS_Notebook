# Core Data Stack
Core Data栈(Stack）是一个框架对象的集合，这些对象作为初始化Core Data的一部风并且协调应用中的对象和外部数据存储。Core Data栈处理与外部数据存储所有的交互，因此应用能够重点关注业务逻辑。Core Data栈包含三个主要对象：托管对象上下文（NSManagedObjectContex）、持久存储协调器（NSPersistentCoordinator）以及托管对象模型（NSManagedObjectModel）。

访问应用数据优先初始化Core Data栈。栈初始化为Core Data的数据请求和数据创建做准备。下面是创建Core Data栈的示例：

```objective-c
@interface MyDataController : NSObject
 
@property (strong) NSManagedObjectContext *managedObjectContext;
 
- (void)initializeCoreData;
 
@end
 
@implementation MyDataController
 
- (id)init
{
    self = [super init];
    if (!self) return nil;
 
    [self initializeCoreData];
 
    return self;
}
 
- (void)initializeCoreData
{
    NSURL *modelURL = [[NSBundle mainBundle] URLForResource:@"DataModel" withExtension:@"momd"];
    NSManagedObjectModel *mom = [[NSManagedObjectModel alloc] initWithContentsOfURL:modelURL];
    NSAssert(mom != nil, @"Error initializing Managed Object Model");
 
    NSPersistentStoreCoordinator *psc = [[NSPersistentStoreCoordinator alloc] initWithManagedObjectModel:mom];
    NSManagedObjectContext *moc = [[NSManagedObjectContext alloc] initWithConcurrencyType:NSMainQueueConcurrencyType];
    [moc setPersistentStoreCoordinator:psc];
    [self setManagedObjectContext:moc];
    NSFileManager *fileManager = [NSFileManager defaultManager];
    NSURL *documentsURL = [[fileManager URLsForDirectory:NSDocumentDirectory inDomains:NSUserDomainMask] lastObject];
    NSURL *storeURL = [documentsURL URLByAppendingPathComponent:@"DataModel.sqlite"];
 
    dispatch_async(dispatch_get_global_queue( DISPATCH_QUEUE_PRIORITY_DEFAULT, 0), ^(void) {
        NSError *error = nil;
        NSPersistentStoreCoordinator *psc = [[self managedObjectContext] persistentStoreCoordinator];
        NSPersistentStore *store = [psc addPersistentStoreWithType:NSSQLiteStoreType configuration:nil URL:storeURL options:nil error:&error];
        NSAssert(store != nil, @"Error initializing PSC: %@\n%@", [error localizedDescription], [error userInfo]);
    });
}
```

## 托管对象模型（NSManagedObjectModel）
`NSManagedObjectModel`描述Core Data即将要访问的数据。在Core Data栈创建期间，作为栈创建的第一步，`NSManagedObjectModel`(通常称为“mom”)被加载到内存中。依赖于应用的结构，可能但是不常见，`NSPersistentStoreCoordinator`协调多个持久存储（persistent store）。

鉴于托管对象模型定义了数据结构，持久存储协调器把持久存储中的数据转换成对象并且传递给请求中的托管对象上下文。持久存储协调器同时验证持久存储中的数据和托管对象模型中定义的是否相匹配。

需要注意的是调用把`NSPersistentStore`添加到`NSPersistentStoreCoordinator`是异步执行的。有几种情形可能引发调用阻碍调用线程（尤其和iCloud和迁移继承）。因此最好异步调用避免阻碍UI线程。

## 托管对象上下文（NSManagedObjectContext）
托管对象上下文是应用与之交互最多的对象，因此它被暴露给应用的其它部分。把托管对象想成是一个智能便签。当从持久存储获取对象时，携带对象的临时拷贝到这个便签上形成一个对象图。然后你可以随意更改这些对象。除非真的保存这些更改，否则持久存储会保持不变。

所有的托管对象都必须使用托管对象上下文注册。使用上下文添加对象到对象图，从对象图移除对象。上下文追踪独立对象的属性和对象与对象之间的关系变化。通过追踪更改，上下文能够提供撤销和回退操作。如果更改对象之间的关系，它还确保维护对象图的完整性。

如果你选择保存更改，上下文确保对象是否在有效状态。如果是，更改被写进持久存储，创建的新对象记录被添加，删除对象的记录被移除。

不使用Core Data，需要写支持数据归档和解档的方法，追踪模型对象的，和撤销管理器交互支持撤销操作。在Core Data框架中，这些功能中的大部分都通过上下文对象自动支持。

**[下一节 : 创建和存储托管对象](/Storage/CoreData/官方文档翻译/创建和存储托管对象.md)**








