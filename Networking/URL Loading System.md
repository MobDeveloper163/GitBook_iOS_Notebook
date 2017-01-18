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

URL Loading类提供两个帮助类来提供附件的metadata，一个用于请求本身，一个用于服务器响应（NSURLResponse）。



