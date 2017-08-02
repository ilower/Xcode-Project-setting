# Xcode 工程常用选项说明和常见问题解决

## 一、Xcode 工程常用选项##

先说和工程配置密切相关的两个概念：**Target** 和 **Configuration**

**Target：**在 Xcode 中创建一个项目时会有一个默认的 Target，我的理解就是编译和构建的单位。同一个项目中允许存在多个 Target，所有的项目配置，源码编译（compile），构建（build)，以及最后打包，都是相对于某个 Target 而言的。一个项目多个 Target 的主要目应该还是为了复用源码、资源以及项目配置，同时方便实现不同 Target 的差异化——例如开发用的 Target 不用集成账号管理系统的 SDK，但提交 App Store 的 Target 需要将 SDK 相关的源码和库文件集成进来，同时还需要在项目配置中指定版本号以及和发布用的签名证书。

**Configuration:** 我的理解就是配置模式，创建一个项目时默认会有两个模式——Debug 和 Release，分别对应开发过程和 App 正式发布的配置内容。Build 时选择不同的配置模式可以实现不同的效果，例如最简单的在Debug配置中开启Log输出，而 Release 配置中关闭。Configuration 也可以根据实际需要进行增减，例如我们新增了一个 "Inhouse" 的 Configuration 用来构建使用企业证书的安装包，便于内部测试。



### 1.General###

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

### 2.Capabilities###

顾名思义，就是 App 可以引入的苹果内置功能，例如内购（**IAP**, **In-App Purchase**），**Game Center** 等。

开启后 Xcode 会自动在项目中引入相关的 Framework。

### 3.Info###

#### *Custom iOS Target Properties*####

#### *URL Types*####

### 4.Build Settings###

显示模式，默认 Combined（可选 Level）

#### *Basic*####

##### *Architextures*#####

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

在 **Basic** 的基础上多了一些配置项，包括以下这些

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

### 5.Build Phases###

#### *Target Dependencies*####

#### *Compile Sources*####

#### *Link Binery With Libraries*####

#### *Run Script*####

#### *Copy Bundle Resources*####

### 6.Build Rules###



