`NSURLSession`类和其相关的类提供了下载内容的API.此类提供了丰富的代理方法集合以支持验证并且在应用没有运行时，或者在iOS中处于挂起状态时，也能够在后台下载。

`NSURLSession`类原生支持`data`，`file`，`ftp`，`http`和`https`。

使用`NSURLSession`API,应用可能创建一个或者多个`sessions`,每一个`session`协调一组相关的数据传输任务。比如，在写一个`web`浏览器，应用可能每一个标签页或者窗口创建一个`session`，或者为交互中的用户创建一个`session`并为后台下载创建另一个`session`。在每一个session中应用为其添加一系列的任务，每一个任务代表一个指定URL请求。

一个URLSession中的任务共享一个配置对象，配置对象定义了连接行为，比如同时连接到同一个服务器的连接的最大数量，是否允许蜂窝网络连接，等等。会话的行为部分由创建其配置对象时调用的方法决定：  

      * 单例共享的`session`(没有配置对象)用于创建基本的请求。它不能够自定以。但是如果是非常局限的请求，使用它是个非常不错的开始。调用`sharedSession`访问这个session。有关其限制的更多信息，请参阅该方法的讨论。

      *  临时会话与默认会话类似，但不要将缓存，`Cookie`或凭据写入磁盘。 您可以通过调用`NSURLSessionConfiguration`类中的`ephemeralSessionConfiguration`方法来创建临时会话配置。

      *  后台会话允许应用在未运行时在后台执行内容的上传和下载。 您可以通过调用`NSURLSessionConfiguration`类中的`backgroundSessionConfiguration:`方法来创建后台会话配置。`NSURLSession`类和其相关的类提供了下载内容的API.此类提供了丰富的代理方法集合以支持验证并且在应用没有运行时，或者在iOS中处于挂起状态时，也能够在后台下载。


