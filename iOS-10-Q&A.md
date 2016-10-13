###Q&A:

1、升级Xcode8 iOS10.0以后RSA等加密出现问题

[解决方法](http://www.cocoachina.com/bbs/read.php?tid=1695854)

2、推送新增文件的介绍  entitlements 

[介绍](http://www.jianshu.com/p/3bb79bdaf87a)

3、ATS issue

iOS 9中默认非HTTPS的网络是被禁止的，当然我们也可以把NSAllowsArbitraryLoads设置为YES禁用ATS。不过iOS 10从2017年1月1日起苹果不允许我们通过这个方法跳过ATS，也就是说强制我们用HTTPS，如果不这样的话提交App可能会被拒绝。但是我们可以通过NSExceptionDomains来针对特定的域名开放HTTP可以容易通过审核。


4、iOS 10 隐私权限问题
**iOS 10 开始对隐私权限更加严格，如果你不设置就会直接崩溃，现在很多遇到崩溃问题了，一般解决办法都是在`info.plist`文件添加对应的`Key-Value`就可以了。**

![](http://ocnhrgfjb.bkt.clouddn.com/ios10%20%E6%9D%83%E9%99%90.png)

**以上`value`值内容不限 但不能为空 最好是可以准确表达的字段  因为要展示给用户看的**

资料 [iOS 10隐私权限](http://www.jianshu.com/p/616240463a7a)


5、Xcode 8 运行一堆没用的logs解决办法

**toolbar 中  editschememe -》 run -》arguments -》environment variables**


**添加 `OS_ACTIVITY_MODE` value为 `disable`;**

6、Xcode 8 插件不能用的问题

[让你的 Xcode8 继续使用插件](http://vongloo.me/2016/09/10/Make-Your-Xcode8-Great-Again/?utm_source=tuicool&utm_medium=referral) 


7、iOS判断版本号的问题

在你的app中不要再使用如下方式检查iOS系统版本

```objc
#define IsIOS7 ([[[[UIDevice currentDevice] systemVersion] substringToIndex:1] intValue]>=7)
```
这会始终返回NO，substringToIndex:1在SDK‘iOS 10.0’(Xcode)中等于SDK‘iOS 1.0’

[建议使用这个demo](https://github.com/BaihaoTian/someWayToJudgePlatformVersion/tree/master/someWayToJudgePlatformVersion)



8、UICollectionView新特性

**前言**
关于 iOS 10 UICollectionView的新特性，主要还是体现在如下3个方面

* 顺滑的滑动体验
现在基本上人人都离不开手机，手机的app也每天都有人在用。一个app的好坏由它的用户体验决定。在可以滑动的视图里面，必须要更加丝滑柔顺才能获得用户的青睐。这些UICollectionView的新特性可以让你们的app比原来更加顺滑，而且这些特性只需要你加入少量的代码即可达到目的。
* 针对self-sizing的改进
self-sizing的API在iOS8的时候被引进，iOS10中加入更多特性使cell更加容易去适配。
* Interactive reordering重排
这个功能在iOS9的时候介绍过了，苹果在iOS 10的API里面大大增强了这一功能。

[UICollectionView新特性](http://www.jianshu.com/p/e97780a24224)