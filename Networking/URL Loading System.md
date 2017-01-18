# URL Loading System

URL Loading System是一个类和协议的集合，允许应用访问URL指向的内容。这项技术的核心是NSURL类，使你的应用能够操作URL以及这个它们指向的内容。

Foundation框架提供了丰富的类集合，使你能加载URL的内容，向服务器上传数据，管理Cookie存储，控制响应缓存，使用应用特定的方式处理凭证存储和授权，以及自定义协议扩展。

URL系统使用以下协议为访问资源提供支持：
* 文件传输协议（ftf://）
* 超文本传输协议（http://）
* 超文本传输编码协议（https://）
* 本地文件URL(file://)
* 数据URL（data://）

URL Loading System包含一些重要的帮助类，URL Loading Classes一起工作。这些主要的帮助类分成五类：协议支持，授权和证书，Cookie存储，配置管理和缓存管理。

![](/assets/url_loading.png)

## URL Loading
在URL Loading系统中最常用的类，允许应用获取URL的内容。你可以使用NSURLSession获取内容。你具体使用的方法大部分依赖于你希望拉取数据到内存还是硬盘。

获取内容转换为data（在内存中）

**有两种获取URL data的方式：**

* 简单的请求，比如NSData对象或者磁盘上的文件，直接使用NSURLSession对象从URLSession获取内容

*更复杂的请求，比如上传数据，比如为NSURLSession提供一个NSURLRequest对象（或者可变的子类，NSMutableURLRequest）

不管使用哪种方式，你的应用都有两种获取相应数据的方式：
* 提供一个完成处理block.The URL Loading类从服务器获取完数据后会调用这个blcok。
* 提供自定义的代理。The URL Loading类在从数据源获取数据时周期性的调用代理的方法。如果需要，你的应用负责累积数据。

除了数据本身，URL Loading类还为完成block和代理提供一个响应对象，响应对象包含与请求相关的metadata，比如MIME TYPE和内容长度。

** 下载内容转换为文件 **

* 简单的请求，比如NSData对象或者磁盘上的文件，直接使用NSURLSession对象从URLSession获取内容

*更复杂的请求，比如上传数据，比如为NSURLSession提供一个NSURLRequest对象（或者可变的子类，NSMutableURLRequest）

> 注意：使用NSURLSession对象启动的下载不会被缓存。如果你需要缓存结果，你的应用必须使用NSURLSession并自行把数据写进磁盘。

## Helper Classes

URL Loading类提供两个帮助类来提供附件的metadata，一个用于请求(NSURLRequest)本身，一个用于服务器响应（NSURLResponse）。

### URL Request
NSURLRequest对象以协议无关的方式封装URL和任何协议特定的属性。

> 注意：当客户端应用程序使用NSMutableURLRequest实例启动连接或下载时，会对请求进行深度复制。 在初始化下载后，对启动请求所做的更改不起作用。

一些协议支持协议特定的属性。 例如，HTTP协议向NSURLRequest添加返回HTTP请求正文，标题和传输方法的方法。 它还添加了NSMutableURLRequest的方法来设置这些值。

### Response Metadata

从服务器到请求的响应可以被看作两部分：描述内容的元数据和内容数据本身。 大多数协议通用的元数据由NSURLResponse类封装，由MIME类型，预期的内容长度，文本编码（如果适用）和提供响应的URL组成。 NSURLResponse的协议特定子类可以提供额外的元数据。 例如，NSHTTPURLResponse存储Web服务器返回的头和状态代码。

> 要点：只有响应的元数据存储在NSURLResponse对象中。 各种URL加载类通过完成处理程序块或对象的委托向您的应用程序提供响应数据本身。
NSCachedURLResponse实例封装了NSURLResponse对象，URL内容数据以及应用程序提供的任何其他信息。

### 重定向和其他请求更改

某些协议（如HTTP）为服务器提供了一种方式，告诉您的应用内容已移至其他网址。 当这种情况发生时，URL加载类可以通知他们的代理。 如果您的应用提供了适当的委托方法，您的应用可以决定是否遵循重定向，从重定向返回响应主体，或返回错误

### 身份验证和凭据
某些服务器限制对某些内容的访问，要求用户通过提供某种类型的凭证（客户端证书，用户名和密码等）进行身份验证，以便获得访问权限。 在Web服务器的情况下，受限内容被分组到需要单组凭证的领域。 凭据（特别是证书）也用于确定在其他方向的信任 - 以评估您的应用是否应该信任服务器。

URL加载系统提供了对证书和保护区域进行建模以及提供安全凭证持久性的类。 在应用启动期间，您的应用可以在单个请求指定这些凭据，或在用户的钥匙串中持续永久地提供凭据。

NSURLCredential类封装了由认证信息（例如用户名和密码）和持久性行为组成的凭证。 NSURLProtectionSpace类表示需要特定凭据的区域。保护空间可以限于单个URL，包含Web服务器上的领域或引用代理。

NSURLCredentialStorage对象管理会话的凭据存储，并提供NSURLCredential对象到相应的NSURLProtectionSpace对象的映射，为此它提供身份验证。只有身份验证质询成功时才会存​​储凭据。

NSURLAuthenticationChallenge类封装了NSURLProtocol实现所需的用于认证请求的信息：建议的凭证，涉及的保护空间，用于确定需要认证的协议的错误或响应，以及已经进行的认证尝试的数量。 NSURLAuthenticationChallenge实例还指定启动身份验证的对象。引发对象，称为发送者，必须符合NSURLAuthenticationChallengeSender协议。

NSURLAuthenticationChallenge实例由NSURLProtocol子类用来通知URL加载系统，需要认证。它们还被提供给NSURLSession的委托方法，以便于自定义认证处理。

### 缓存管理
