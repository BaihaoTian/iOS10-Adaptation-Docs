##User Notifications


![](http://ocnhrgfjb.bkt.clouddn.com/image/iOS10NotificationShare/000.png)

----------

###前言

####为什么是［通知］而不是［推送］

先来看一下iOS10通知相关的第一个更新点就是新加了一个框架`User Notification Framework`，从字面翻译来看应该翻译成“用户通知框架”，而通常大家所了解的“推送”翻译成英文应该是“Push”，“Push”其实只是［通知］触发的一种方式，而［通知］其实是操作系统层面的一种UI展示。

在苹果的官方文档中Notification分为两类：

* Remote（远程，也就是之前所说的Push的方式）

* Local（本地，通知由本地的事件触发，iOS10中有三种不同的触发‘Trigger’方式，稍后会进行详细说明）

所以，［推送］只是［通知］的一种触发方式，而从iOS迭代更新的历史特征中看，［通知］应该是是被苹果作为一个重点内容来延展的。（从最初的单纯展示和简单回调，到Backgroud的支持，再后来整体的Payload的长度由256字节扩展到2K再到4K，再看这次的独立框架还有丰富的特性更新）


----------


###更新点（概览）

**由User Notification Framework 整合通知相关方法看特性变化**

通知相关的方法由之前一直存在在UIKit Framework中到独立出来，官方确实做了很多，但是也尽量做到让开发者可以平滑的过度。

原文：

>1.Familiar API with feature parity

>2.Expanded content

>3.Same code path for local and remote notification handling

>4.Simplified delegate methods

>5.Better notification management

>6.In-app presentation option

>7.Schedule and handle notifications in extensions

>8.Notification Extensions


释义：

>1.相同的特性使用类似的API（之前的功能API使用方法类似但是还是稍有改变）

>2.内容扩展（支持附件和展示更多内容）

>3.本地通知和远程通知操作代码在相同调用路径（合并代理方法）

>4.简化代理方法

>5.更好的通知管理（支持通知查、改、删；增强本地通知管理，增加日历与地理位置事件的触发）

>6.应用内通知展示（之前App在前台的情况下收到通知不会UI展示）

>7.在Extensions中规划和操作通知（使更新通知内容和删除误发或过期的通知内容成为可能，另一个重要场景为端到端加密）

>8.引入通知Extensions



----------

###从产品&运营的角度来看更新点

####增加Subtitle

Subtitle样式和展示位置如下图所示，Subtitle的加入给内容类App带来了福音，交给优秀编辑和策划去使用应该是一项利器。

![](http://ocnhrgfjb.bkt.clouddn.com/image/iOS10NotificationShare/111.png)


####增加Attachments

通过类似之前的content_available参数的mutable-content参数来控制是否增加Attachments，需要开发者实现NotificationServiceExtension来展示带有Attachments的通知，需要注意的一点是，本地通知的话只能使用本地的资源，远程通知需要服务端发送URL给NotificationServiceExtension去预先执行下载操作，当然如果在网络不太通畅的情况下苹果也提供了超时时间和超时之后的后续操作让开发者在这种情况下也能适当的展示通知，从而提高通知交互体验。

Attachments的加入也可以让你更好的对发给用户的通知进行分类。

![](http://ocnhrgfjb.bkt.clouddn.com/image/iOS10NotificationShare/222.png)

####新增本地通知Triggers

在本地通知新增了两种新的Triggers，就是日历和地理位置。日历的话就是让开发者可以根据指定的日期和时间来展示本地通知，并且支持循环条件，比如“每周二上午十一点”这种条件。地理位置的话就是在进入或者离开指定区域来触发这条本地通知，该特性让iOS通知的地理围栏触发有了实现的可能，比如“某品牌App在你进入该品牌线下店铺的范围内即展示最新优惠信息”等。

典型场景：

* 循环提醒
* 地理围栏

![](http://ocnhrgfjb.bkt.clouddn.com/image/iOS10NotificationShare/333.png)

####内容扩展显示

如果设备支持3DTouch的话用力按压通知即可进入内容扩展页面，此页面会可以由开发者自定义展示内容，可以是之前Attachments的内容比如图片视频，也可以是开发者自己定义的布局内容，同时也支持在内容扩展页面增加更多的自定义ActionButton。但是，个人认为有一些遗憾的是扩展内容几乎不支持交互，交互就只能放到ActionButton里面了。

![](http://ocnhrgfjb.bkt.clouddn.com/image/iOS10NotificationShare/444.png)
![](http://ocnhrgfjb.bkt.clouddn.com/image/iOS10NotificationShare/555.png)


####通知查、改、删

实现该功能需要有一个必要参数就是构建通知的identifer，后续的查改删操作都是根据此参数去执行的。

典型的应用场景：

* 赛事比分变更
* 通知撤回

![](http://ocnhrgfjb.bkt.clouddn.com/image/iOS10NotificationShare/666.png)

![](http://ocnhrgfjb.bkt.clouddn.com/image/iOS10NotificationShare/777.png)

####增加Service Extension

让App开发者可以在展示通知之前增加一层处理逻辑，从而使端到端加密成为可能，也就意味着经由苹果的服务器的通知内容可以是完全的密文，在这之前iOS上实现通知内容加密是没有任何可能的。

典型应用场景：

* 端到端加密
* 添加Attachments

![](http://ocnhrgfjb.bkt.clouddn.com/image/iOS10NotificationShare/888.png)

####获取通知相关设置信息API

可以通过API获取到用户设置该App相关于推送通知的设置的详细列表，该信息的统计可以让App的开发者更好的根据用户的通知使用习惯来改进通知的策略。
![](http://ocnhrgfjb.bkt.clouddn.com/image/iOS10NotificationShare/999.png)

####增加应用内通知展示API

提供官方的应用内收到APNs通知并做UI展示的API，在此之前如果想做此类功能需要开发者自己开发功能，此API的优势在于让开发者更简单的实现应用内展示通知的功能并且统一点击通知之后的事件。
![](http://ocnhrgfjb.bkt.clouddn.com/image/iOS10NotificationShare/100.png)





----------
###从开发的角度来看更新点

####UserNotifications framework详解
首先介绍UserNotification的几个主要类
![](http://ocnhrgfjb.bkt.clouddn.com/image/iOS10NotificationShare/userNotificationClass.jpeg)


[1]`UNNotification`主要是作为通知`delegate`方法的参数使用。包含`UNNotificationRequest`信息。


[2]`UNNotificationAction`是通知中添加的`action`，展示在通知栏的下方。默认以的`button`样式展示。有一个文本输入的子类`UNTextInputNotificationAction`。可以在点击`button`之后弹出一个键盘，输入信息。用户点击信息和输入的信息可以在`UNNotificationResponse`中获取。

[3]`UNNotificationAttachment`是新增的通知内容格式，可以设置图像和视频。

[4]`UNNotificationCategory`是通知样式类型。在注册通知之后，展示通知之前，可以自定义通知样式，并使用
`[[UNUserNotificationCenter currentNotificationCenter] setNotificationCategories:[NSSetsetWithObject:categoryNotification]]`
设置到通知中心中。根据通知内容中的`categoryIdentifier`使用不同的通知样式。这里需要注意：使用自定义的通知操作按钮和通知`Content`可以设置为同一个`category`。

[5]`UNNotificationContent`通知的主体内容，原通知的`title`，`sound`，`badge`和新的`attachments`，`lacnchImageName`都在这里进行设置，是创建一个通知的前提。

[6]`UNNotificationRequest`通知请求。当通知内容，触发条件都准备好之后，需要包装为一个通知请求，由通知中心来激活这个通知。

[7]`UNNotificationResponse`通知响应。作为通知的`action`被用户触发之后，App可以拿到的信息。和`action`对应，有普通的`UNNotificationResponse`和子类`UNTextInputNotificationResponse`。其中包括`action`的`identifier`和完整的`UNNotification`。子类`UNTextInputNotificationResponse`还包含`userText`，用户输入的内容。

[8]`UNNotificationServiceExtension`是一个extension。用户可以在收到特性的通知时，一般是远程，并且该远程通知的apns中包含一个`mutable-content`字段，值为1。


你有30秒的时间处理这个通知，可以同步下载图像和视频到本地，然后包装为一个UNNotificationAttachment扔给通知，这样就能展示用服务器获取的图像或者视频了。这里需要注意：如果数据处理失败，超时，extension会报一个崩溃信息，但是通知会用默认的形式展示出来，app不会崩溃。

[9]UNNotificationSetting schedule notifications that involve user interactions such as displaying alerts, playing sounds, or badging the app’s icon.。

[10]UNNotificationSound通知的声音。可以直接使用声音的name，而不是文件路径。

[11]UNNotificationTrigger通知的触发条件。

* UNTimeIntervalNotificationTrigger
* UNCalendarNotificationTrigger
* UNLocationNotificationTrigger

[12]UNUserNotificationCenter通知中心。最主要的类，通知的注册，激活，编辑，删除等功能都由该类完成。


####Content
以前只能展示一条文字，现在可以有 title 、subtitle 以及 body 了。
![](http://ocnhrgfjb.bkt.clouddn.com/image/iOS10NotificationShare/111.png)
定制方法如下：

```objc
//Local Notification
UNMutableNotificationContent *content = [[UNMutableNotificationContent alloc] init];
content.title = @"Introduction to Notifications";
content.subtitle = @"Session 707";
content.body = @"Woah! These new notifications look amazing! Don’t you agree?";
content.badge = @1;

//Remote Notification
{
"aps" : {
    "alert" : { 
         "title" : "Introduction to Notifications", 
         "subtitle" : "Session 707",         
         "body" : "Woah! These new notifications look amazing! Don’t you agree?"
                },
    "badge" : 1
        },
}
```


####新增本地通知Triggers

又是一个新的功能，有三种

* UNTimeIntervalNotificationTrigger
* UNCalendarNotificationTrigger
* UNLocationNotificationTrigger

```objc
//2 分钟后提醒
UNTimeIntervalNotificationTrigger *trigger1 = [UNTimeIntervalNotificationTrigger triggerWithTimeInterval:120 repeats:NO];

//每小时重复 1 次喊我喝水
UNTimeIntervalNotificationTrigger *trigger2 = [UNTimeIntervalNotificationTrigger triggerWithTimeInterval:3600 repeats:YES];

//每周二早上 10：00 提醒我闪电购粉丝节抽奖
NSDateComponents *components = [[NSDateComponents alloc] init];
components.weekday = 2;
components.hour = 8;
UNCalendarNotificationTrigger *trigger3 = [UNCalendarNotificationTrigger triggerWithDateMatchingComponents:components repeats:YES];

//#import <CoreLocation/CoreLocation.h>
//一到公司就提醒我买闪电购
CLRegion *region = [[CLRegion alloc] init];
UNLocationNotificationTrigger *trigger4 = [UNLocationNotificationTrigger triggerWithRegion:region repeats:NO];

```
 发送通知
 
 ```objc
 
 NSString *requestIdentifier = @"sampleRequest";
UNNotificationRequest *request = [UNNotificationRequest requestWithIdentifier:requestIdentifier
                                                                          content:content
                                                                          trigger:trigger1];
[center addNotificationRequest:request withCompletionHandler:^(NSError * _Nullable error) {

}];
 ```



####Notification Handling
设定了推送，然后就结束了？iOS 10 并没有这么简单！
通过实现协议，使 App 处于前台时捕捉并处理即将触发的推送：

```objc
@interface AppDelegate () <UNUserNotificationCenterDelegate>

-(void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions))completionHandler{

    completionHandler(UNNotificationPresentationOptionAlert | UNNotificationPresentationOptionSound);

}
```
让它只显示 alert 和 sound ,而忽略 badge 。


#### Notification Management
**彻底掌控整个推送周期：**

* Local Notification 通过更新 request
* Remote Notification 通过新的字段 apns-collapse-id
通过之前的 `addNotificationRequest:` 方法，在 id 不变的情况下重新添加，就可以刷新原有的推送。

```objc
NSString *requestIdentifier = @"sampleRequest";
UNNotificationRequest *request = [UNNotificationRequest requestWithIdentifier:requestIdentifier
                                                                      content:newContent
                                                                      trigger:newTrigger1];
[center addNotificationRequest:request withCompletionHandler:^(NSError * _Nullable error) {

}];
```

删除计划的推送：

```objc
[center removePendingNotificationRequestsWithIdentifiers:@[requestIdentifier]];

```
此外 UNUserNotificationCenter.h 中还有诸如删除所有推送、查看已经发出的推送、删除已经发出的推送等等强大的接口。

#### service extension  [详细资料](http://www.jianshu.com/p/f77d070a8812)

![](http://static.zybuluo.com/coderXu/dvmhjtokav23et22r60sd2ry/Extension01.png)

从这张图上，我们可以看到，原先直接从APNs推送到用户手机的消息，中间添加了ServiceExtension处理的一个步骤（当然你也可以不处理），通过这个ServiceExtension，我们可以把即将给用户展示的通知内容，做各种自定义的处理，最终，给用户呈现一个更为丰富的通知，当然，如果网络不好，或者附件过大，可能在给定的时间内，我们没能将通知重新编辑，那么，则会显示发过来的原版通知内容。

那么ServiceExtension可以做什么呢？它的意义是什么呢？我总结了几点：

1、 安全
安全总是摆在第一位的，从iOS9开始，苹果鼓励我们使用更为安全的https协议可以看的出来，苹果公司是对安全很重视的一家公司，为什么在这里我会提到安全呢？是因为之前我们的推送内容，不管是通过第三方，还是通过苹果自带的通知处理，如果让有心人对数据做一次拦截，抓个包啥的，我们推送的内容就会完全暴露，当然有的同学说，我可以加密啊~但是不知道大家有没有想过，如果数据加密，那通知栏会怎么展示呢？（你千万别跟我说你把所有的远程推送变成本地通知。。）通过此次这次增加的UNNotificationServiceExtension的类，便可以更好的帮助我们实现数据的加密。
它的原理便是在收到通知后的最多30s内，你可以把你的通知内容，解密后，在重新展示在用户的通知拦上。

2、 内容的丰富
之前的通知展示内容比较少，以至于被各种广告提醒占据了。这次苹果新添加的附件通知，结合上通知拓展类，便可以给用户展现出一个有着丰富内容的通知。比如，一个小短片的某一秒的画面啊（这里要强烈鄙视各大平台的某些电影预览图），又或者是配上一些小图片啊（通过服务器传来的imaUrl）等方式来吸引用户，诱导用户点开你的通知，促使用户会使用你的App。其实推送这个功能，虽然有的人会关闭，但是大部分的人还是开启的，所以说推送这个市场还是很大的哟~灵活利用推送，会让你的程序拥有无限的可能。

**添加 Service Extension**

先在 Xcode 打开你的 App 工程，File - New - Target 然后添加notificationService

然后会自动创建一个 UNNotificationServiceExtension 的子类 NotificationService，通过完善这个子类，来实现你的需求。

点开 NotificationService.m 会看到 2 个方法：

```objc
- (void)didReceiveNotificationRequest:(UNNotificationRequest *)request withContentHandler:(void (^)(UNNotificationContent * _Nonnull))contentHandler {
    self.contentHandler = contentHandler;
    self.bestAttemptContent = [request.content mutableCopy];

    self.bestAttemptContent.title = [NSString stringWithFormat:@"%@ [modified]", self.bestAttemptContent.title];

    self.contentHandler(self.bestAttemptContent);
}

- (void)serviceExtensionTimeWillExpire {
    self.contentHandler(self.bestAttemptContent);
}
```


* didReceiveNotificationRequest 让你可以在后台处理接收到的推送，传递最终的内容给 contentHandler
* serviceExtensionTimeWillExpire 在你获得的一小段运行代码的时间即将结束的时候，如果仍然没有成功的传入内容，会走到这个方法，可以在这里传肯定不会出错的内容，或者他会默认传递原始的推送内容


####获取通知相关设置信息API

之前注册推送服务，用户点击了同意还是不同意，以及用户之后又做了怎样的更改我们都无从得知，现在 apple 开放了这个 API，我们可以直接获取到用户的设定信息了。

```objc
[center getNotificationSettingsWithCompletionHandler:^(UNNotificationSettings * _Nonnull settings) {
        NSLog(@"%@",settings);
}];

```

打印获得如下信息：

```objc
<UNNotificationSettings: 0x16567310; 
authorizationStatus: Authorized, 
notificationCenterSetting: Enabled, 
soundSetting: Enabled, 
badgeSetting: Enabled, 
lockScreenSetting: Enabled, 
alertSetting: NotSupported,
carPlaySetting: Enabled, 
alertStyle: Banner>
```

####服务器payload 格式 
加入title 和 subtitle

```
{
  "aps":{
    "alert":{
      "title":"I am title",
      "subtitle":"I am subtitle",
      "body":"I am body"
    },
    "sound":"default",
    "badge":1
  }
}
```

可被serviceExtension拦截的格式 需要有 mutable-content 字段

```
{
    "aps": {
        "alert": "This is some fancy message.",
        "badge": 1,
        "sound": "default",
        "mutable-content": "1",
        "imageAbsoluteString": "http://upload.univs.cn/2012/0104/1325645511371.jpg"

   }
}
```

