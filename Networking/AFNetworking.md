### AFURLSessionManager

AFURLSessionManager遵从<NSURLSessionTaskDelegate>, <NSURLSessionDataDelegate>, <NSURLSessionDownloadDelegate>和 <NSURLSessionDelegate>，基于指定的NSURLSessionConfiguration对象创建和管理NSURLSession.

### 创建一个下载任务

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

### 创建一个上传任务

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

### 创建一个带进度的、断点续传任务

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






