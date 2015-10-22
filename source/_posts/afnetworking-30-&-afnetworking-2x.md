title: 'AFNetworking 3.0 & AFNetworking 2.X'
date: 2015-10-22 13:44:09
tags: [第三方库,iOS]
---

AFNetworking是一款在OS X和iOS下都令人喜爱的网络库。为了迎合iOS新版本的升级, AFNetworking在3.0版本中删除了基于 NSURLConnection API的所有支持。如果你的项目以前使用过这些API，建议您立即升级到基于 NSURLSession 的API的AFNetworking的版本。本指南将引导您完成这个过程。

本指南是为了引导使用AFNetworking 2.x升级到最新的版本API，以达到过渡的目的，并且解释了新增和更改的设计结构。

<!-- more -->

###新设备要求: iOS 7, Mac OS X 10.9, watchOS 2, tvOS 9, & Xcode 7

AFNetworking 3.0正式支持的iOS 7， Mac OS X的10.9， watchOS 2 ， tvOS 9 和Xcode 7。如果你想使用AFNetworking在针对较旧版本的SDK项目，请检查README的兼容性信息。

###NSURLConnection的API已废弃

AFNetworking 1.0建立在NSURLConnection的基础API之上 ，AFNetworking 2.0开始使用NSURLConnection的基础API ，以及较新基于NSURLSession的API的选项。 AFNetworking 3.0现已完全基于NSURLSession的API，这降低了维护的负担，同时支持苹果增强关于NSURLSession提供的任何额外功能。由于Xcode 7中，NSURLConnection的API已经正式被苹果弃用。虽然该API将继续运行，但将没有新功能将被添加，并且苹果已经通知所有基于网络的功能，以充分使NSURLSession向前发展。

AFNetworking 2.X将继续获得关键的隐患和安全补丁，但没有新的功能将被添加。Alamofire(Swift下的网络请求)软件基金会建议，所有的项目迁移到基于NSURLSession的API。

###弃用的类

下面的类已从AFNetworking 3.0中废弃：

- AFURLConnectionOperation
- AFHTTPRequestOperation
- AFHTTPRequestOperationManager

###修改的类

下面的类包含基于NSURLConnection的API的内部实现。他们已经被使用NSURLSession重构:

- UIImageView+AFNetworking
- UIWebView+AFNetworking
- UIButton+AFNetworking

迁移

AFHTTPRequestOperationManager 核心代码

如果你以前使用 AFHTTPRequestOperationManager ， 你将需要迁移去使用 AFHTTPSessionManager。 以下的类在两者过渡间并没有变化：

- securityPolicy
- requestSerializer
- responseSerializer

接下来举一个关于AFHTTPSessionManager的简单例子。注意HTTP网络请求返回的不再是AFHTTPRequestOperation, 修改成为了NSURLSessionTask，并且成功和失败的Block块中的参数也变更为了NSURLSessionTask，而不再是AFHTTPRequestOperation。

AFNetworking 2.x

```
AFHTTPRequestOperationManager *manager = [AFHTTPRequestOperationManager manager];
[manager GET:@"请求的url" parameters:nil success:^(AFHTTPRequestOperation *operation, id responseObject) {
NSLog(@"成功");
} failure:^(AFHTTPRequestOperation *operation, NSError*error) {
NSLog(@"失败");
}];
```

AFNetworking 3.0

```
AFHTTPSessionManager *session = [AFHTTPSessionManager manager];
[session GET:@"请求的url" parameters:nil success:^(NSURLSessionDataTask *task, id responseObject) {
NSLog(@"成功");
} failure:^(NSURLSessionDataTask *task, NSError *error) {
NSLog(@"失败");        
}];
```

###AFHTTPRequestOperation 核心代码

与NSURLConnection对象不同，每个共享应用范围的设置如会话管理、缓存策略、Cookie存储以及URL协议等，这些NSURLSession对象都可以单独进行配置。使用特定的配置来初始化会话，它可以发送任务来获取数据，并上传或下载文件。

在AFNetworking 2.0中，使用AFHTTPRequestOperation，有可能创建一个没有额外开销的独立的网络请求来获取数据。NSURLSession则需要更多的开销，为了获得所要请求的数据。

接下来，将要通过AFHTTPSessionManager创建一个单例，并创建一个任务和启动它。

AFNetworking 2.x
```
NSURL *URL = [NSURL URLWithString:@""];
NSURLRequest *request = [NSURLRequest requestWithURL:URL];
AFHTTPRequestOperation *op = [[AFHTTPRequestOperation alloc] initWithRequest:request];
op.responseSerializer = [AFJSONResponseSerializer serializer];
[op setCompletionBlockWithSuccess:^(AFHTTPRequestOperation *operation, id responseObject) {
NSLog(@"JSON: %@", responseObject);
} failure:^(AFHTTPRequestOperation *operation, NSError *error) {
NSLog(@"Error: %@", error);
}];
[[NSOperationQueue mainQueue] addOperation:op];
```

AFNetworking 3.0

```
NSURL *URL = [NSURL URLWithString:@""];
AFHTTPSessionManager *manager = [AFHTTPSessionManager manager];
[manager GET:URL.absoluteString parameters:nil success:^(NSURLSessionTask *task, id responseObject) {
NSLog(@"JSON: %@", responseObject);
} failure:^(NSURLSessionTask *operation, NSError *error) {
NSLog(@"Error: %@", error);
}];
```

###UIKit的迁移

图片下载已经被重构，以遵循AlamofireImage架构与新的AFImageDownloader类。这个类的图片下载职责的代理人是UIButton与UIImageView的类目，并且提供了一些方法，在必要时可以自定义。类别中，下载远程图片的实际方法没有改变。

UIWebView的类目被重构为使用AFHTTPSessionManager作为其网络请求。

###UIAlertView的类目被废弃

从AFNetworking 3.0后UIAlertView的类目因过时而被废弃。并没有提供UIAlertController类目的计划，因为这是应用程序应处理的逻辑，而不是这个库。

