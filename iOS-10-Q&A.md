###Q&A:

####1、升级Xcode8 iOS10.0以后RSA等加密出现问题

[解决方法](http://www.cocoachina.com/bbs/read.php?tid=1695854)

----------

####2、推送新增文件的介绍  entitlements 

[介绍](http://www.jianshu.com/p/3bb79bdaf87a)

----------

####3、ATS issue

iOS 9中默认非HTTPS的网络是被禁止的，当然我们也可以把NSAllowsArbitraryLoads设置为YES禁用ATS。不过iOS 10从2017年1月1日起苹果不允许我们通过这个方法跳过ATS，也就是说强制我们用HTTPS，如果不这样的话提交App可能会被拒绝。但是我们可以通过NSExceptionDomains来针对特定的域名开放HTTP可以容易通过审核。

安全传输不再支持SSLv3, 建议尽快停用SHA1和3DES算法

----------


####4、iOS 10 隐私权限问题
**iOS 10 开始对隐私权限更加严格，如果你不设置就会直接崩溃，现在很多遇到崩溃问题了，一般解决办法都是在`info.plist`文件添加对应的`Key-Value`就可以了。**

![](http://ocnhrgfjb.bkt.clouddn.com/ios10%20%E6%9D%83%E9%99%90.png)

**以上`value`值内容不限 但不能为空 最好是可以准确表达的字段  因为要展示给用户看的**

资料 [iOS 10隐私权限](http://www.jianshu.com/p/616240463a7a)

----------

####5、Xcode 8 运行一堆没用的logs解决办法

**toolbar 中  editschememe -》 run -》arguments -》environment variables**


**添加 `OS_ACTIVITY_MODE` value为 `disable`;**

----------

####6、Xcode 8 插件不能用的问题

[让你的 Xcode8 继续使用插件](http://vongloo.me/2016/09/10/Make-Your-Xcode8-Great-Again/?utm_source=tuicool&utm_medium=referral) 

----------


####7、iOS判断版本号的问题

在你的app中不要再使用如下方式检查iOS系统版本

```objc
#define IsIOS7 ([[[[UIDevice currentDevice] systemVersion] substringToIndex:1] intValue]>=7)
```
这会始终返回NO，substringToIndex:1在SDK‘iOS 10.0’(Xcode)中等于SDK‘iOS 1.0’

[建议使用这个demo](https://github.com/BaihaoTian/someWayToJudgePlatformVersion/tree/master/someWayToJudgePlatformVersion)

----------


####8、UICollectionView新特性

**前言**
关于 iOS 10 UICollectionView的新特性，主要还是体现在如下3个方面

* 顺滑的滑动体验
现在基本上人人都离不开手机，手机的app也每天都有人在用。一个app的好坏由它的用户体验决定。在可以滑动的视图里面，必须要更加丝滑柔顺才能获得用户的青睐。这些UICollectionView的新特性可以让你们的app比原来更加顺滑，而且这些特性只需要你加入少量的代码即可达到目的。
* 针对self-sizing的改进
self-sizing的API在iOS8的时候被引进，iOS10中加入更多特性使cell更加容易去适配。
* Interactive reordering重排
这个功能在iOS9的时候介绍过了，苹果在iOS 10的API里面大大增强了这一功能。

[UICollectionView新特性](http://www.jianshu.com/p/e97780a24224)

----------


####9.色彩API
(1)在iOS10中，UIColor类中新增加了两个方法，用来创建sRGB模式的色彩。与RGB相比，sRGB是更加标准的色彩模式，RGB色彩在不同设备上可能存在颜色偏差，sRGB则更加精准但同时色域范围也更窄一些。UIColor中新添加的方法如下：


```objc
//类方法创建sRGB模式色彩
+ (UIColor *)colorWithDisplayP3Red:(CGFloat)displayP3Red green:(CGFloat)green blue:(CGFloat)blue alpha:(CGFloat)alpha NS_AVAILABLE_IOS(10_0);
//初始化方法创建sRGB模式色彩
- (UIColor *)initWithDisplayP3Red:(CGFloat)displayP3Red green:(CGFloat)green blue:(CGFloat)blue alpha:(CGFloat)alpha NS_AVAILABLE_IOS(10_0);
```


(2)全局的设置色彩风格

一般情况下，iOS系统会根据用户所在环境的光线进行屏幕色彩的调节，在iOS10系统中，开发者可以在info.plist文件中全局的配置色彩风格来设置外界光线对APP内色彩的影响程度。

在info.plist文件中可以添加如下键：

White Point Adaptivity Style

这个键可以设置的值列举如下：

Standard White Point Adaptivity Style  标准色彩模式

Reading White Point Adaptivity Style   阅读色彩模式

Photo White Point Adaptivity Style      照片色彩模式

Video White Point Adaptivity Style      视频色彩模式

Game White Point Adaptivity Style      游戏色彩模式

上面几种模式从上到下，对色彩的保真度依次提高。

----------


####10.XIB和Storeboard适配
在Xcode8之前，创建一个XIB或SB文件，都是一个600*600的方块XIB文件。在Xcode8之后，创建的XIB文件默认是6s尺寸的大小。

但是Xcode8打开之前旧项目的XIB或SB文件时，会弹出下面的弹框， 这时候一般直接选择Choose Device即可。

![](http://ocnhrgfjb.bkt.clouddn.com/image/iOS10NotificationShare/103.jpeg)

但是这样有个问题，如果Xcode8打开过这个XIB文件，并选择Choose Device之后。其他的Xcode8以下版本的编译器，将无法再打开这个文件，会报以下错误：

> The document “ViewController.xib” requires Xcode 8.0 or later. This version does not support documents saved in the Xcode 8 format. Open this document with Xcode 8.0 or later.

有两种方法解决这个问题：

* 你同事也升级Xcode8，比较推荐这种方式，应该迎接改变。
* 右击XIB或SB文件 -> Open as -> Source Code，删除xml文件中下面一行字段。

```
<code>
  <capability name="documents saved in the Xcode 8 format" mintoolsversion="8.0"/>
</code>
```

----------


####11.编译错误

升级Xcode之后，Xcode8对之前的一些修饰符和语句不兼容，会导致一些编译错误。这种错误导致的原因很多，这里大致列几条，各位还是根据自身遇到的情况做修改吧。

* 之前一些泛型相关的修饰符，nullable之类的有的会报错。

* CAAnimation及其子类，设置代理属性后，必须在@interface()遵守代理，否则报错，等等。

----------


####12.awakeFromNib报警告

老项目在Xcode8中，有些重写awakeFromNib方法的地方，会报下面的错误。这是因为没有调用super的方法导致的

----------


####13.log新选项
可以通过在Arguments中设置参数，打印出App加载的时长，包括整体加载时长，动态库加载时长等。
在Environment Variables中添加DYLD_PRINT_STATISTICS字段，并设置为YES，在控制台就会打印加载时长。
![](http://ocnhrgfjb.bkt.clouddn.com/image/iOS10NotificationShare/104.jpeg)