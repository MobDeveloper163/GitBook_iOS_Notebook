# 在iOS启动时集成Core Data

iOS和OS X应用的生命周期在启动时，有细微的差别。

当OS X花费很长的时间启动，不响应时，操作系统改变鼠标指示其状态。用户从而可以决定等待应用启动完毕还是关闭应用。

在iOS中，没有这样的概念。如果应用在有限的时间内没有完成启动，操作系统就会终止这个应用。因此应用尽快完成启动程序是必不可少的。

另一方面，你又希望尽可能快的访问Core Data中的数据，通常意味着要在应用声明周期的第一步中初始化Core Data。但是Core Data偶尔会比平常花费更长的时间初始化。

因此，建议iOS启动顺序分成两部分以避免应用终止：
1.    以最小的启动项提示用户应用正在启动；
2.    一旦Core Data完成其初始化，应用UI完成加载。

## 在iOS中初始化Core Data
第一步是更改`application:didFinishLaunchingWithOptions:`方法的实现。考虑在`application:didFinishLaunchingWithOptions:`中初始化Core Data以及其它非常小的事情。如果用的`storyboard`，在执行这个方法期间，可以继续显示启动图片。

作为初始化Core Data的一部分，在后台队列中，添加持久存储（`NSPersistentStore`）到持久存储协调（NSPersistentStoreCoordinator）中。这个操作花费的时间长短不能确定，如果放在主线程中可能会阻塞用户界面，可能导致应用被终止。

一旦持久存储添加到持久存储协调器中，你可以再回到主队列，请求完成用户界面并展示给用户。

## 把Core Data从应用代理中分开

之前在iOS中，Core Data栈通常都是在AppDelegate中初始化。然而，这样做创建了大量的代码和应用代理事件一起变得很混乱。

建议Core Data栈自己的顶级控制器对象中创建，应用代初始化这个控制器，并且持有这个控制器的引用。此操作将促进在自己的控制器中整合Core Data代码，并保持应用程序委托相对干净。 

```
@interface AppDelegate : UIResponder <UIApplicationDelegate>
 
@property (strong, nonatomic) UIWindow *window;
@property (strong, nonatomic) DataController *dataController;
 
@end
 
@implementation AppDelegate
 
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{
    [self setDataController:[[DataController alloc] init];
    // Basic User Interface initialization
    return YES;
}
```

在初始化分离的控制器时传入一个完成块，已经把Core Data栈移除了应用代理。但是任然允许应用代理对其回调，这样用户界面能够知道什么时候拉取数据。



