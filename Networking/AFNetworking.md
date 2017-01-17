## AFNetworking
### 安装
使用CocoaPods安装，在Podfile中配置如下：
> source 'https://github.com/CocoaPods/Specs.git'

> platform :ios, '8.0'

> pod 'AFNetworking', '~> 3.0'

然后，在终端运行
> $ pod install

### 结构体系

#### NSURLSession
* AFURLSessionManager
* AFHTTPSessionManager

#### 序列化
* **AFURLRequestSerialization**
	* AFHTTPRequestSerialization
	* AFJSONSerialization
	* AFPropertityListSerialization
	
	
* **AFURLResponseSerialization**
	* AFHTTPResponseSerialization
	* AFJSONResponseSerialization
	* AFXMLParserResponseSerialization
	* AFPropertyListResponseSerialization
	* AFImageResponseSerialization
	* AFCompoundResponseSerialization
	
#### 附加功能
* AFSecurityPolicy
* AFNetworkReachabilityManager

### 使用
#### AFURLSessionManager
AFURLSessionManager创建并管理一个基于指定NSURLSessionConfiguration对象的NSURLSession对象，遵从NSURLSessionTaskDelegate, NSURLSessionDataDelegate, NSURLSessionDownloadDelegate 和NSURLSessionDelegate协议

##### 创建一个下载任务

```
NSURLSessionConfiguration *configuration = [NSURLSessionConfiguration defaultSessionConfiguration];
AFURLSessionManager *manager = [[AFURLSessionManager alloc] initWithSessionConfiguration:configuration];

NSURL *URL = [NSURL URLWithString:@"http://example.com/download.zip"];
NSURLRequest *request = [NSURLRequest requestWithURL:URL];

NSURLSessionDownloadTask *downloadTask = [manager downloadTaskWithRequest:request progress:nil destination:^NSURL *(NSURL *targetPath, NSURLResponse *response) {
	NSURL *documentsDirectoryURL = [[NSFileManager defaultManager] URLForDirectory:NSDocumentDirectory inDomain:NSUserDomainMask appropriateForURL:nil create:NO error:nil];
	return [documentsDirectoryURL URLByAppendingPathComponent:[response suggestedFilename]];
} completionHandler:^(NSURLResponse *response, NSURL *filePath, NSError *error) {
	 NSLog(@"File downloaded to: %@", filePath);
}];

[downloadTask resume];

```

##### 创建一个上传任务

```
NSURLSessionConfiguration *configuration = [NSURLSessionConfiguration defaultSessionConfiguration];
AFURLSessionManager *manager = [[AFURLSessionManager alloc] initWithSessionConfiguration:configuration];

NSURL *URL = [NSURL URLWithString:@"http://example.com/upload"];
NSURLRequest *request = [NSURLRequest requestWithURL:URL];

NSURL *filePath = [NSURL fileURLWithPath:@"file://path/to/image.png"];

NSURLSessionUploadTask *uploadTask = [manager uploadTaskWithRequest:request fromFile:filePath progress:nil completionHandler:^(NSURLResponse *response, id responseObject, NSError *error) {
	 if (error) {
		 NSLog(@"Error: %@", error);
	 } else {
		 NSLog(@"Success: %@ %@", response, responseObject);
	 }

}];

[uploadTask resume];

```

##### 创建一个带进度的、断点续传任务

```
NSMutableURLRequest *request = [[AFHTTPRequestSerializer serializer] multipartFormRequestWithMethod:@"POST" URLString:@"http://example.com/upload" parameters:nil constructingBodyWithBlock:^(id<AFMultipartFormData> formData) {

 [formData appendPartWithFileURL:[NSURL fileURLWithPath:@"file://path/to/image.jpg"] name:@"file" fileName:@"filename.jpg" mimeType:@"image/jpeg" error:nil];

 } error:nil];

AFURLSessionManager *manager = [[AFURLSessionManager alloc] initWithSessionConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration]];

NSURLSessionUploadTask *uploadTask = [manager uploadTaskWithStreamedRequest:request progress:^(NSProgress * _Nonnull uploadProgress) {

 // This is not called back on the main queue.
 // You are responsible for dispatching to the main queue for UI updates
	dispatch_async(dispatch_get_main_queue(), ^{ //Update the progress view	
		[progressView setProgress:uploadProgress.fractionCompleted];
	 });
 } completionHandler:^(NSURLResponse * _Nonnull response, id _Nullable responseObject, NSError * _Nullable error) {
	 if (error) {
		 NSLog(@"Error: %@", error);
	 } else {
		 NSLog(@"%@ %@", response, responseObject);
	 }
}];

[uploadTask resume];

```

##### 创建一个data task

```
NSURLSessionConfiguration *configuration = [NSURLSessionConfiguration defaultSessionConfiguration];
AFURLSessionManager *manager = [[AFURLSessionManager alloc] initWithSessionConfiguration:configuration];

NSURL *URL = [NSURL URLWithString:@"http://httpbin.org/get"];
NSURLRequest *request = [NSURLRequest requestWithURL:URL];

NSURLSessionDataTask *dataTask = [manager dataTaskWithRequest:request completionHandler:^(NSURLResponse *response, id responseObject, NSError *error) {
    if (error) {
        NSLog(@"Error: %@", error);
    } else {
        NSLog(@"%@ %@", response, responseObject);
    }
}];
[dataTask resume];
```
### 请求序列化

请求序列化程序从URL字符串创建请求，将参数编码为查询字符串或HTTP正文。
```
NSString *URLString = @"http://example.com";
NSDictionary *parameters = @{@"foo": @"bar", @"baz": @[@1, @2, @3]};
```

#### 请求字符串参数编码
```
[[AFHTTPRequestSerializer serializer] requestWithMethod:@"GET" URLString:URLString parameters:parameters error:nil];
```
> GET http://example.com?foo=bar&baz[]=1&baz[]=2&baz[]=3

#### URL表单参数编码
```
[[AFHTTPRequestSerializer serializer] requestWithMethod:@"POST" URLString:URLString parameters:parameters error:nil];
```
> POST http://example.com/

> Content-Type: application/x-www-form-urlencoded

> foo=bar&baz[]=1&baz[]=2&baz[]=3

#### JSON参数编码
```
[[AFJSONRequestSerializer serializer] requestWithMethod:@"POST" URLString:URLString parameters:parameters error:nil];
```
> POST http://example.com/

> Content-Type: application/json

> {"foo": "bar", "baz": [1,2,3]}

#### Network Reachability Manager(网络可达性管理)
AFNetworkReachabilityManagerj监听域名、WWAN和Wifiw网络接口的地址是否可访问。
* 不要使用Reachability决定原始的请求是否应该发送。
	* 应该尝试发送请求。
* 可以使用Reachability决定什么时候应该重新发送请求。
	* 尽管请求还会失败，连接可用的通知是再次尝试请求的好时机。
* Reachability是检测请求为什么失败的好工具
	* 网络请求失败后，在告诉用户已离线比告诉他们一个准确的技术性的错误要好，比如“请求超时”

#### SHARED NETWORK REACHABILITY
```
[[AFNetworkReachabilityManager sharedManager] setReachabilityStatusChangeBlock:^(AFNetworkReachabilityStatus status) {
    NSLog(@"Reachability: %@", AFStringFromNetworkReachabilityStatus(status));
}];

[[AFNetworkReachabilityManager sharedManager] startMonitoring];
```
### 安全策略
AFSecurityPolicy通过安全连接评估针对固定的X.509证书和公用密钥的服务器信任。

将固定的SSL证书添加到您的应用程序有助于防止中间人攻击和其他漏洞。 强烈建议处理敏感客户数据或财务信息的应用程序通过配置和启用SSL绑定的HTTPS连接路由所有通信。

#### 允许无效的证书
```
AFHTTPSessionManager *manager = [AFHTTPSessionManager manager];
manager.securityPolicy.allowInvalidCertificates = YES; // 生产环境不推荐
```

### 单元测试
AFNetworking包含单元测试套件，放在在Tests子文件目录中。这些测试可以简单地在要测试的平台框架上执行测试操作来运行。
