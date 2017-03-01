## [AFNetwork解读](http://www.cnblogs.com/jx66/p/5685625.html)

### 常量写法规范

* .h中

```
extern NSString * const AFNetworkingReachabilityDidChangeNotification;

extern NSString * const AFNetworkingReachabilityNotificationStatusItem;
```

* .m中对应的

```
NSString * const AFNetworkingReachabilityDidChangeNotification = @"com.alamofire.networking.reachability.change";

NSString * const AFNetworkingReachabilityNotificationStatusItem = @"AFNetworkingReachabilityNotificationStatusItem";
```

* 特殊写法

```
.h中
extern NSString * AFStringFromNetworkReachabilityStatus(AFNetworkReachabilityStatus status);

.m中
NSString * AFStringFromNetworkReachabilityStatus(AFNetworkReachabilityStatus status) {

 switch (status) {

 case AFNetworkReachabilityStatusNotReachable:

 return NSLocalizedStringFromTable(@"Not Reachable", @"AFNetworking", nil);

 case AFNetworkReachabilityStatusReachableViaWWAN:

 return NSLocalizedStringFromTable(@"Reachable via WWAN", @"AFNetworking", nil);

 case AFNetworkReachabilityStatusReachableViaWiFi:

 return NSLocalizedStringFromTable(@"Reachable via WiFi", @"AFNetworking", nil);

 case AFNetworkReachabilityStatusUnknown:

 default:

 return NSLocalizedStringFromTable(@"Unknown", @"AFNetworking", nil);
 }
}
```

### 私有方法写法

* 我们在开发中经常会写成`- (void)funcName;` 这样的私有方法。
* 建议是一个类中的私有方法写成`static void funcName()` 这样的c函数比较好。

  * 1. 在文件的最前方，比较容易查找

  * 1. 可以适当的使用内联函数，提高效率



### [ARC下OC对象和CF对象之间的桥接\(bridge\)](http://www.cnblogs.com/zzltjnh/p/3885012.html)

`__bridge,__bridge_transfer,__bridge_retained`

* 1.\_\_bridge:CF和OC对象转化时只涉及对象类型不涉及对象所有权的转化；
  ```
  NSURL *url = [[NSURL alloc] initWithString:@"http://www.baidu.com"];
  CFURLRef ref = (__bridge CFURLRef)url;
  ```

* 2.\_\_bridge\_transfer:常用在讲CF对象转换成OC对象时，将CF对象的所有权交给OC对象，此时ARC就能自动管理该内存；（作用同CFBridgingRelease\(\)  代表ARC负责自动释放该对象,不需要我们去release
* 3.\_\_bridge\_retained:（与\_\_bridge\_transfer相反）常用在将OC对象转换成CF对象时，将OC对象的所有权交给CF对象来管理；\(作用同CFBridgingRetain\(\)\)  代表OC要将对象所有权交给CF对象自己来管理,所以我们要在ref使用完成以后用CFRelease将`其手`动释放.

  ```
  NSURL *url = [[NSURL alloc] initWithString:@"http://www.baidu.com"];
  CFURLRef ref = (__bridge_retained CFURLRef)url;
  CFRelease(ref);
  ```


### 此类的核心方法,设置监听网咯监听

` startMonitoring `



* 1.我们先新建上下文

* 2.设置回调

* 3.加入RunLoop池




### KVO之注册键值依赖

* 举例,比如有个**Card类,**


### [NSSet 小结](http://blog.csdn.net/ms2146/article/details/8657011)



