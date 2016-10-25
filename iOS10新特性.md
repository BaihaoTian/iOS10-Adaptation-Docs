##iOS10 新特性 


![](https://onevcat.com/assets/images/2016/ios-10-title.png)


随着WWDC16的结束，iOS版本号也跨入了两位数 10.0。对于开发者来说，好消息是iOS10中并没有加入太多内容。相比于开疆扩土，iOS 10 更专注的是对现有内容的改进，以弥补之前迅速发展所留下的一些问题，这其实正是 Apple 当下所亟需做的事情。

###生态整合与 Extension 开发

>在 iOS 10 里 Apple 延续了前几年的策略，那就是进行平台整合。全世界现在没有另外一家厂商在掌握了包括桌面，移动到穿戴的一系列硬件设备的同时，还掌控了相应的从操作系统，到应用软件，再到软件商店这样一套完整的布局。Apple 显然也非常明白这个优势意味着什么。所以近年来 Apple 一直强调平台整合，如果你的应用能够同时在 iOS，watchOS 以及 macOS 上工作的话，毫无疑问将会更容易吸引用户以及 Apple 的喜爱。

>另外一点则是各个应用之间的整合和交互。不难发现，随着近年来 extension 开发的兴起，Apple 逐渐在从 app 是“用户体验的核心”这个理念中转移，变为用户应该也可以在通知中心，桌面挂件或者手表这样的地方完成必要交互。而应用之间的交互在以前可以说是 iOS 系统的禁区，但是去年随着 Workflow 的成功，Apple 对于应用之间的交互有助于用户生产力的提升有了清晰的认识。今年 SDK 中几个重大更新其实都是围绕这个主题来进行的。

#####1、Extension 是什么？
 
首先我们得问，Extension 是什么？这里可能会有很多不同的解释。
 
一个很明显的例子是便是使用了相同术语的 Firefox 和 Chrome 的 Extension，这两大浏览器提供了非常丰富的 Extension 用于游览器本身以及增强 Web 浏览体验。确切地说，这里的 Extension 是一种 Plugin。
 
还有一个是 Android 的 Intent，Intent 使 App 可以实现相关 Intent，在系统或者一个 App 里，用户可以自由的选择另一个 App 去完成特定的任务。所以，Android 的 Intent 是一种 IPC（应用程序间的通讯）。
 
这里的分类有点极端，毕竟浏览器有真正的 Plugin，如 Flash 插件；Android 也有提供 Widget，也更接近系统的 Plugin。在这点上，我可以说 iOS/OS X 的 Extension 也是混合了上面提到的一切：既有 Plugin 的部分（如 Today），也有 IPC 的部分（如 Share、Photo Editing）。
 
于是让我们就 Extension 简单地理解为：增强系统默认功能，能让 App 之间的协同工作成为可能。

[Extension 基础](http://www.cocoachina.com/ios/20140620/8891.html)

#####2、SiriKit

SiriKit
Siri API 的开放自然是 iOS 10 SDK 中最激动人心也是亮眼的特性。SiriKit 为我们提供一全套从语音识别到代码处理，最后向用户展示结果的流程。Apple 加入了一套全新的框架 Intents.framework 来表示 Siri 获取并解析的结果。你的应用需要提供一些关键字表明可以接受相关输入，而 Siri 扩展只需要监听系统识别的用户意图 (intent)，作出合适的响应，修改以及实际操作，最后通过 IntentsUI.framework 提供反馈。整个过程非常清晰明了，但是这也意味着开发者所能拥有的自由度有限。

在 iOS 10 中，我们只能用 SiriKit 来做六类事情，分别是：

* 语音和视频通话

* 发送消息

* 发送或接收付款

* 搜索照片

* 约车

* 管理健身

如果你的应用恰好正在处理这些领域的问题的话，添加 Intents Extension 的支持会是很棒的选择。它将提高用户使用你的应用的可能性，也能让用户在其他像是地图这样的系统级应用中使用你的服务。

Siri和Maps通过Intents extension的扩展方式和我们的应用进行交互，其中，类型为INExtension的对象扮演着Intents extension扩展中直接协同Siri对象共同响应用户请求的关键角色。当我们实现了Intents extension扩展并产生了一个Siri请求事件时，一个典型的Intent事件的处理过程中总共有这三个步骤Resolve、Confirm和Handle：

* Resolve阶段。在Siri获取到用户的语音输入之后，生成一个INIntent对象，将语音中的关键信息提取出来并且填充对应的属性。这个对象在稍后会传递给我们设置好的INExtension子类对象进行处理，根据子类遵循的不同服务protocol来选择不同的解决方案

* Confirm阶段。在上一个阶段通过handler(for intent:)返回了处理intent的对象，此阶段会依次调用confirm打头的实例方法来判断Siri填充的信息是否完成。匹配的判断结果包括Exactly one match、Two or more matches以及No match三种情况。这个过程中可以让Siri向用户征求更具体的参数信息

* 在confirm方法执行完成之后，Siri进行最后的处理阶段，生成答复对象，并且向此intent对象确认处理结果然后执显示结果给用户看


![](http://cc.cocimg.com/api/uploads/20160621/1466516499797031.jpg)

[资料](http://www.cocoachina.com/ios/20160622/16785.html)



#####3、iMessage Apps
Message 应用大概是 Apple 在宣传 iOS 10 时着力最多的部分了。虽然新的贴纸包，自动转换颜文字，发送全屏效果等功能都很酷炫，但是对于程序开发者来说，可能还是对 iMessage Apps 更感兴趣。Xcode 8 中，Apple 在 iOS Application 模板中添加了一类新的项目类型，Messages Application。同时，模拟器甚至还开发了新的双人对话模式，以供开发者调试这类 app。

虽然名义上是独立 app，但实际上工作的依然是一个 extension。在该扩展中，Messages.framework 将承担与系统的 message 界面交互的主要职责。你通过提供一个自定义的 View Controller，来获取用户在使用你的 message app 时进行对话的上下文，以及发送接收等操作，并做出合适的响应。这个扩展在用来进行直接在 Message 应用中一些自定义共享会很好玩。但是鉴于 Apple 暂时没有打算将 Message.app 跨平台的原因，可能也注定了这只会是一种补充，而无法成为主流。

[资料](http://www.jianshu.com/p/be79b8729bf8)


#####4、User Notifications
通知中心向来是 iOS 上的兵家必争之地。如何提供适时有效的通知，往往决定了用户活跃和留存的可能性。在 iOS 10 上，Apple 对通知进行了加强和革新。现在，为了更好地处理和管理通知，和本地及推送通知相关的 API 被封装到了全新的框架 UserNotifications.framework 中。在 iOS 10 中，开发者的服务器有机会在本地或者远程通知发送给用户之前再进行修改。

另外，在之前加入了 notification action 以及 text input 的基础上，iOS 10 又新增了为通知添加音频，图片，甚至视频的功能。现在，你的通知不仅仅是提醒用户回到应用的入口，更成为了一个展示应用内容，向用户传递多媒体信息的窗口。

之后会详解。






###IDE 和工具改进



##### Xcode 8


在 app 签名方面，Apple 终于意识到了他们在 Xcode 7 中所犯得错误。我想可能不止一个人被证书和描述文件出问题时的 "Fix Issue" 按钮坑过。这个按钮不仅不会修正问题，反而会直接注销现有的开发者证书，然后“自作主张”地重新申请。大多数情况下，这让事情变得更加糟糕。特别是对于新加入的开发者，他们并不理解 Apple 的证书系统，错误的操作和处置，往往让开发环境变得不可挽回。Xcode 8 中，同一个开发者帐号现在允许多个开发证书，而完全重做的 app 签名系统也足够好用，并且避免了误操作的可能性。在兼顾自动配置的基础上，也为大型项目和复杂的 CI 环境提供了足够灵活的配置空间，这绝对值得点赞。

另外 Xcode 终于提供了进行代码编辑器扩展的能力。现在开发者可以创建 XCSourceEditorExtension 来对 Xcode 的功能进行扩展了，在没有文档帮助和官方支持的情况下摸索着为 Xcode 制作插件的历史也即将结束。

另外开放了Runtime Issues的使用。在开发过程中，因为语法或明显的代码错误(例如Retain Cycle)，编译器可以发现并报黄色或红色警告。但是一些因为代码逻辑导致的错误，编译器并没有办法找到。这时候可以通过Xcode8提供的Runtime Issues新特性，查找到运行过程中出现的问题，并通过Graph的方式将问题可视化的展现给开发者。

* Thread Sanitizer spots:新的线程污点清理器, 解决多线程情况下的资源竞争条件,数据的变化和其它相关线程的bug

* View Debugger:使用更新的带有更大的保真度和视觉精度检查UI约束问题的视图调试器
* Memory Debugger:可以用新的内存调试跟踪器跟踪发出的内存泄漏警报。

![](http://ocnhrgfjb.bkt.clouddn.com/image/iOS10NotificationShare/102.jpg)


#####Swift 3
Swift 开源已经过去半年时间。在 Swift 2.2 中我们已经看到了开源的社区力量对语言产生的深刻影响，而在 Swift 3 中这一影响的效果将更加明显。

最大的变化在于 Foundation 框架的重新导入，可能过一段时间再回头看的话，这将标志着 Swift 与 Objective-C 彻底分家。Foundation 框架中的 API 现在以更符合 Swift 的方式被导入到语言中。大体来说，这些变化包括去除 `NS` 前缀，将绝大部分 class 转换为 struct (虽然底层还是 copy-on-write 的引用实现，可以参看 `ReferenceConvertible` 协议的内容)，去掉 API 中重复的语义等。如果在当前你还能看出 Swift 和 Objective-C 在使用 Foundation 或者说开发 app 时同根同源的话，Swift 3 正式发布后可能情况会大不相同。

由于引用类型向值类型的转换，也将导致我们在使用 Swift 开发时的思考方式发生变化。以往的 Foundation 框架中类型的可变性是由不可变类型和它的可变类型版本 (比如 `NSData` 和 `NSMutableData`) 来进行区分的。而在 Swift 3 中，一般来说将只有作为结构体的不可变类型 (比如 `Data`)，对于这类结构体的改变，将会是更安全的基于写时复制的行为，而不再是原来可变对象那样的危险的内存操作。这在很多时候除了保证数据共享时的安全性以外，内部的引用特性也保证了调用速度。实际上，因为减少了不必要的复制 (比如根据一个不可变对象创建相应的可变对象)，实际上通过 Swift 3 的 API 使用 Foundation 的速度将比原来更快！



