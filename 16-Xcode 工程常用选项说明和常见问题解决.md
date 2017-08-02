# Xcode 工程常用选项说明和常见问题解决

## 一、Xcode 工程常用选项##

先说两个概念：Target 和 Configuration（默认是 Debug 和 Rlease）

### 1.General###

#### *Identity*####

*Bundle Identifier:*

*Version:* 对应 CFBundleShortVersionString

*Build:* 对应 CFBundleVersion，和 Version 的区别在于，每次提交审核时必须比上一次的版本号要高，而 Version 可以不变。

*Team:*

#### *Deployment Info*####

*Deployment Target:*

*Devices:*

*Device Orientation:*

*Status Bar Style:*

#### *Linked Frameworks and Library*####

### 2.Capabilities###

顾名思义，就是 app 需要的苹果的内置功能支持，例如内购（**IAP**, **In-App Purchase**），**Game Center** 等

开启后 Xcode 会自动在工程中引入相关的 Framework。

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



