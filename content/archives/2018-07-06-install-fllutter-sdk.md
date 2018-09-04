---
title: "安装 Flutter SDK"
date: 2018-07-06T10:17:34+08:00
Tags: ["flutter"]
categories: ["flutter"]
---

The Flutter SDK includes Flutter’s engine, framework, widgets, tools, and a Dart SDK.   
Flutter SDK 包括 Flutter 的引擎、框架、widgets、工具和 Dart SDK。

> 以 macOS 系统为例

#### 1、配置国内的镜像站点

国内用户建议配置一个与官方同步的可信的镜像站点，帮助 Flutter 命令行工具到该镜像站点下载其所需的资源，来加速安装和使用，为此需要设置两个环境变量：“PUB_HOSTED_URL”和“FLUTTER_STORAGE_BASE_URL”，然后再运行 Flutter 命令行工具。配置方法如下：

```
export PUB_HOSTED_URL=https://pub.flutter-io.cn
export FLUTTER_STORAGE_BASE_URL=https://storage.flutter-io.cn
```

目前可用的镜像站点有两个：

* 上海交通大学 Linux 用户组  
FLUTTER_STORAGE_BASE_URL: https://mirrors.sjtug.sjtu.edu.cn  
PUB_HOSTED_URL: https://dart-pub.mirrors.sjtug.sjtu.edu.cn  

* Flutter 社区  
FLUTTER_STORAGE_BASE_URL: https://storage.flutter-io.cn  
PUB_HOSTED_URL: https://pub.flutter-io.cn  

更多信息请翻阅：[Using Flutter in China](https://github.com/flutter/flutter/wiki/Using-Flutter-in-China) | [Flutter 社区中文资源-使用镜像](https://flutter-io.cn/#section-china-mirror)  

#### 2、安装 Flutter SDK

安装方式有两种，一种是直接 clone 官方仓库 `git clone -b beta https://github.com/flutter/flutter.git`；另一种是下载官方安装包(installation bundle)

##### 2.1 use git clone

创建要保存 Flutter SDK 的目录后直接 clone 即可，通常都是使用 beta 分支：

```
git clone -b beta https://github.com/flutter/flutter.git
```

后续的步骤请参考 2.2 节的 `Step2: 配置环境变量`。

##### 2.2 download installation bundle

浏览 [SDK archive](https://flutter.io/sdk-archive/) 查看所有可用的版本。


![flutter-sdk](https://lollipop.xiaosongfu.com/notes/software/201807/flutter-sdk.png)

Flutter SDK 分 Beta channel、Dev channel、Master channel。Beta channel 包含大多数稳定的 Flutter 构建。

Beta channel 最稳定；其次是 Dev channel；Master channel 分支极不稳定，有时会有破坏性修改。相关信息请翻阅 [Flutter’s channels](https://github.com/flutter/flutter/wiki/Flutter-build-release-channels)。

浏览 [Installation bundles](https://github.com/flutter/flutter/wiki/Flutter-Installation-Bundles)查看官方安装包(installation bundle)是怎么组织的。

* Step1: 下载并解压 bundle

在 [SDK archive](https://flutter.io/sdk-archive/) 页面下载bundle，是一个 zip 压缩包，将其解压到你想要保存 Flutter SDK 的目录。

> 这个下载地址需要 FQ ，可以到笔者搭建的 Mirrors 站点下载：[https://mirrors.xiaosongfu.com](https://mirrors.xiaosongfu.com/#/flutter/flutter)

* Step2: 配置环境变量

使用 vi 编辑 $HOME/.bash_profile，加入以下内容

```
export PUB_HOSTED_URL=https://pub.flutter-io.cn
export FLUTTER_STORAGE_BASE_URL=https://storage.flutter-io.cn

export PATH=$PATH:/path/to/flutter/bin
```

`/path/to/flutter` 就是你保存 Flutter 的位置。

#### 3、初始化 Flutter

配置好环境变量后执行 `flutter doctor` ，该命令会下载 Dart SDK，Flutter 的依赖库并且编译 Flutter Tools，还会检查本机的环境是否满足 Flutter 开发，比如 Android SDK 是否配置好、IDE 的 Flutter 插件是否已经安装，等等，检查的结果会显示在命名行，如果有不满足的，按要求修复即可。修复后重新执行 `flutter doctor` 查看修复结果。

某一次的运行结果记录如下：

```
fuxiaosongdeMac-mini:~ fuxiaosong$ flutter doctor
Doctor summary (to see all details, run flutter doctor -v):
[✓] Flutter (Channel beta, v0.5.1, on Mac OS X 10.13.5 17F77, locale zh-Hans-CN)
[✓] Android toolchain - develop for Android devices (Android SDK 27.0.3)
[!] iOS toolchain - develop for iOS devices (Xcode 9.3)
    ✗ Verify that all connected devices have been paired with this computer in Xcode.
      If all devices have been paired, libimobiledevice and ideviceinstaller may require updating.
      To update, run:
        brew uninstall --ignore-dependencies libimobiledevice
        brew install --HEAD libimobiledevice
        brew install ideviceinstaller
[✓] Android Studio (version 2.3)
    ✗ Flutter plugin not installed; this adds Flutter specific functionality.
    ✗ Dart plugin not installed; this adds Dart specific functionality.
[✓] Android Studio (version 3.1)
    ✗ Flutter plugin not installed; this adds Flutter specific functionality.
    ✗ Dart plugin not installed; this adds Dart specific functionality.
[!] IntelliJ IDEA Community Edition (version 2017.1)
    ✗ Flutter plugin not installed; this adds Flutter specific functionality.
    ✗ Dart plugin not installed; this adds Dart specific functionality.
[✓] IntelliJ IDEA Ultimate Edition (version 2017.3.5)
[!] VS Code (version 1.25.0)
[✓] Connected devices (1 available)

! Doctor found issues in 3 categories.
fuxiaosongdeMac-mini:~ fuxiaosong$
```

#### 4、配置 IDE

配置 IDE 很简单，Jetbrains 系的 IDE 只需安装 Dart Plugin 和 Flutter Plugin 即可。 Visual Studio Code 只需要安装好 Dart 和 Flutter 扩展即可。