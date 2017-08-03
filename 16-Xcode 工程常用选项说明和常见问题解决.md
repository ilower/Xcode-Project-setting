# Xcode 项目配置选项说明和常见问题

## 一、Xcode 项目常用选项##

先说和项目配置密切相关的两个概念：**Target** 和 **Configuration**

**Target：**在 Xcode 中创建一个项目时会有一个默认的 Target，我的理解就是编译和构建的单位。同一个项目中允许存在多个 Target，所有的项目配置，源码编译（compile），构建（build)，以及最后打包，都是相对于某个 Target 而言的。一个项目多个 Target 的主要目应该还是为了复用源码、资源以及项目配置，同时方便实现不同 Target 的差异化——例如开发用的 Target 不用集成账号管理系统的 SDK，但提交 App Store 的 Target 需要将 SDK 相关的源码和库文件集成进来，同时还需要在项目配置中指定版本号以及和发布用的签名证书。

**Configuration:** 我的理解就是配置模式，创建一个项目时默认会有两个模式——Debug 和 Release，分别对应开发过程和 App 正式发布的配置内容。Build 时选择不同的配置模式可以实现不同的效果，例如最简单的在Debug配置中开启Log输出，而 Release 配置中关闭。Configuration 也可以根据实际需要进行增减，例如我们新增了一个 "Inhouse" 的 Configuration 用来构建使用企业证书的安装包，便于内部测试。

### 1.General 标签页###

#### *Identity*####

##### *Bundle Identifier:*##### 

是一个 App 的唯一标识，格式就是倒过来的域名字符串，如： **com.MyCompany.MyProductName**。

Bundle ID 和苹果生态系统中其他子系统的关联如下图：

![Bundle ID](http://help.apple.com/xcode/mac/9.0/en.lproj/Art/bundleid_2x.png)

Xcode 项目中，Bundle ID 保存在Info.plist 中，最后会打入软件包中。

Bundle ID 和 App ID 是一对一的关系，iTunes Connect（App提审和发布管理后台）创建 App ID 和 Bundle ID 的对应后不能修改和删除。

iCloud container ID 可以对应多个 Bundle ID，我的理解也就是多个 App 可以共享一个 iCloud 设置。（*）

##### *Version:* 

是一个格式如：1.2.3 的字符串，左起第一位表示主版本号，第二位是次版本号（新特性或重要的修改），最后一位可以是类似 bug 修复的小提交。会在 App Store 的 App 页面显示给用户。

对应 Info.plist 中的 Key: **CFBundleShortVersionString** 。

##### *Build:* 

和 **Version** 的格式含义相同。

二者区别在于，**Build** 版本号每次提交新的审核包时必须比上一次的版本号要高，否则不能通过上传的验证，而 **Version** 可以保持不变。

对应 Info.plist 中的 Key: **CFBundleVersion**。

*其他*：咱们的项目中，**Version** 会指定为底包版本号，而 **Build** 会在打底包脚本中用最新的 svn 版本号替换掉 Xcode 中的设置，这样可以保证每次打新底包都不用关心 **Build** 的设置。如果上传了一个审核包后出现和打包时的 svn 版本号无关的问题（例如：*）需要重新打包，那么就需要再做一个 svn 提交把版本号提高才能改变 **Build** 版本号保证成功上传。

##### *Team:*#####

应该和证书签名（Code  Sign）有关，新版本的 Xcode 已经没有这个配置项，相关信息会在 **Signing** 的配置项中体现。

#### *Deployment Info*####

##### *Deployment Target:*

选择一个可以运行 App 的 iOS系统最低版本，例如：iOS 5.1.1

##### *Devices:*

选择一个目标设备，有 iPad，iPhone，Universal。Universal 表示在 iPad 和 iPhone 上都可以运行。

##### *Device Orientation:*

选择 App 运行时是以垂直（**Portrait**）还是以水平（**LandScape**）的方式显示。

##### *Status Bar Style:*

选择状态条的样式，默认（**Default**）是深色样式（明亮背景），**Light** 是明亮样式（深色背景）

需要隐藏状态栏勾选 "Hide status bar" ，适用全屏显示的 App。

需要全屏显示勾选 "Requires full screen" 。

#### *Linked Frameworks and Library*####

在这个配置下可以增减和项目有关的 Framework 以及静态库文件。



### 2.Capabilities 标签页###

顾名思义，就是 App 可以引入的苹果内置功能，例如内购（**IAP**, **In-App Purchase**），**Game Center** 等。

开启后 Xcode 会自动在项目中引入相关的 Framework。



### 3.Info 标签页###

默认 **Target** 的这个标签页上的配置项实际都保存在 Info.plist 文件中，不同的 **Target** 可以对应不同的 Info.plist 文件。非默认 **Target** 的 Info.plist 文件通常会有和 **Target** 相关的名称，例如用 Info_Release.plist 表示是正式发布用的。

其他：给某个 **Target** 指定专用 plist 文件的设置在 **Build Setting** 标签页中，**Packaging** 选项的 "info.plist File" 子选项。

#### *Custom iOS Target Properties*####

[苹果官方的属性 Key 和 Value列表](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Introduction/Introduction.html)

**Bundle display name**: App 的名称，在 App Store 的介绍页中显示，以及安装后在图标下显示

**Bundle name**：默认的  value 是 **${PRODUCT_NAME}**，PRODUCT_NAME 是在 Build 项目时执行的一个 Shell 脚本中导出的环境变量，它的值实际就是 Xcode 左边栏中的项目名称。类似的 Key 还有 *Executable file*，value 是 ${EXECUTABLE_NAME}。（Tips: 如何查看 Xcode 中所有可用的环境变量？见下图）

![查看 Xcode 环境变量](https://github.com/ilower/Xcode-Project-setting/blob/master/images/show_env_var.png?raw=true)


有几个配置项和 **General** 标签页中的一样，在这两处其中任意一处修改的效果相同。

**Bundle identifier*，*Bundle version*（Build），*Bundle versions string, short*（Version），*Status bar is initially hidden*，*Supported interface orientations**

另外说两个重要的 Key：
**Fonts provided by application**: 指定 App 使用的自定义字体，其 value 是个 array，可以同时指定多个字体文件名称，同时再将自定义字体文件添加到项目的 Resources 目录下就可以在 App 中使用了。

**NSAppTransportSecurity**：[官方说明](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW33)

其 value 是 dictionary，通过添加子项 **NSAllowsArbitraryLoads** （可设置 YES 或 NO）来忽略或遵守 iOS 9 以上系统对所有 HTTP 连接都必须使用 HTTPS 的规则。之前 iOS 系统刚升级到 9.0 时，由于我们项目相关的 http 服务没有来得及支持 HTTPS，所以将 **NSAllowsArbitraryLoads** 临时设置为 YES 来避免 App 默认用 HTTPS 协议导致连接失败的问题。后来所有相关的服务都支持 HTTPS 之后，就把 **NSAllowsArbitraryLoads** 改回 NO，使用更安全的 HTTPS 协议。

#### *URL Types*####

这个配置项设置后，可以在 Objective C 代码中通过一种类似URL请求的方案来检查系统是否安装了某个 App。
实际对应 Info.plist 文件中的 key —— **CFBundleURLTypes**，其 value 是一个 array，array 的每个元素又是一个 dictionary，在 dictionary 设置一个 App 相关的模式字符串。

[官方说明](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CoreFoundationKeys.html#//apple_ref/doc/uid/TP40009249-102207-TPXREF115)   [LSApplicationQueriesSchemes](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/LaunchServicesKeys.html#//apple_ref/doc/uid/TP40009250-SW14)   [canOpenURL](https://developer.apple.com/documentation/uikit/uiapplication/1622952-canopenurl)



### 4.Build Settings 标签页###

这个是编译项目和构建 App 的相关配置，默认 Combined 的显示模式（可选 Level，能看到各配置项目的但感觉没有 Combined 模式下各配置项之间的从属关系明确）。

另外在内容上也分为 **Basic** 和 **All** 两个列表，**All** 在 **Basic** 的基础上多了一些配置项，后面会一一提到。

#### *Basic*####

##### *Architextures*（[参考](http://www.jianshu.com/p/3fce0bd6f045)）#####

###### *Architextures*：

指定项目被编译成可支持哪些处理器指令集类型，同时支持的指令集越多，就会编译出包含多个指令集代码的数据包，对应生成二进制包就越大，也就是ipa包会变大。

常见的有 armv7(32位)，arm64(64位)，armv7s，这些是移动处理器ARM的指令集，开发手机 App 时需要支持。i386 和 x86_64 是Mac处理器的指令集，如果开发一个在 Mac 上运行的 App，就选择这两个处理器架构。

###### *Base SDK*:

指定项目编译时基于的 iOS 的 SDK 名称或路径，以便引用正确的头文件和库文件。这个路径是所有 search paths 的前缀，同时也会作为环境变量传递给编译器和连接器。 

###### *Build Active Architexture Only*:

指定是否只对当前连接调试设备所支持的指令集进行编译。
当连接移动设备进行调试时，设置为YES，就只编译当前设备的 architecture 版本；而设置为 NO 时，会编译 Architextures 选项中设置的所有指令集的版本。 所以，一般联机调试的时候可以选择设置为 YES，可以节约编译时间，出 release 包的时候再改为 NO，以适应不同设备。

###### *Valid Architextures*:

限制可能被支持的指令集的范围，也就是 Xcode 编译出来的二进制包类型最终从这些类型产生，而编译出哪种指令集的包，将由 Architectures 与 Valid Architectures（因此这个不能为空）的交集来确定。

##### *Build Options*#####

##### *Code Signing*#####

##### *Deployment*#####

##### *Linking*#####

##### *Packaging*#####

##### *Search Paths*#####

##### *Apple LLVM 6.1 - Code Generation*#####

##### *Apple LLVM 6.1 - Custom Complier Flags*

##### *Apple LLVM 6.1 - Languages*#####

##### *Apple LLVM 6.1 - Preprocessing*#####

##### *User Definded*#####

#### *All*####

##### *Build Locations*#####

##### *Kernel Module*#####

##### *Testing*#####

##### *Versioning*#####

##### *Apple LLVM 6.1 - Languages - C++*#####

##### *Apple LLVM 6.1 - Languages - Modules*#####

##### *Apple LLVM 6.1 - Languages - Objective C*#####

##### *Apple LLVM 6.1 - Warning Policies*#####

##### *Apple LLVM 6.1 - Warnings - All languages*#####

##### *Apple LLVM 6.1 - Warnings - C++*#####

##### *Apple LLVM 6.1 - Warnings - Objective C*#####

##### *Apple LLVM 6.1 - Warnings - Objective C And ARC*#####

##### *OSACompile - Build Options*#####

##### *Static Analyzer - Analysis Policy*#####

##### *Static Analyzer - Generic Issues*#####

##### *Static Analyzer - Issues - Objective C*#####

##### *Static Analyzer - Issues - Security*#####



### 5.Build Phases 标签页###

#### *Target Dependencies*####

#### *Compile Sources*####

#### *Link Binery With Libraries*####

#### *Run Script*####

#### *Copy Bundle Resources*####



### 6.Build Rules 标签页###



