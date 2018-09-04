---
title: "安装 Dartlang"
date: 2018-06-26T17:54:31+08:00
tags: ["dartlang"]
categories: ["dartlang"]
---

> 
The Dart SDK has the libraries and command-line tools that you need to develop Dart apps.（Dart SDK 包含开发 Dart 应用需要用到的库(libraries) 和 命令行工具(command-line tools)）

有两种安装方式：  
1. 使用操作系统的包管理工具直接安装，如 `brew install dart`，简单方便，具体请参考：[https://www.dartlang.org/tools/sdk](https://www.dartlang.org/tools/sdk)   
2. 下载 archive 包解压并配置好环境变量即可，具体请参考：[https://www.dartlang.org/tools/sdk/archive](https://www.dartlang.org/tools/sdk/archive)

> 以 macOS 系统为例

#### 1. 方式一 使用系统包管理工具安装
使用 homebrew 安装，需要安装 dart 自己的仓库，然后才能使用 `brew install dart` 来安装：

```
$ brew tap dart-lang/dart
$ brew install dart
```

使用 `--devel` 参数安装 `dev channel`：

```
$ brew install dart --devel
```

>
更新版本使用：`brew upgrade dart`  
查看所有 dart 版本：`brew info dart`  
切换 dart 版本：`brew switch dart 1.24.3`  

#### 2. 方式二 下载 archive 包

##### 1.创建保存 Dart 的目录

```
mkdir -p ~/develop/dart
cd ~/develop/dart
```

##### 2.下载 Dart archive 包
到 [https://www.dartlang.org/tools/sdk/archive](https://www.dartlang.org/tools/sdk/archive) 下载所需版本的 archive 包：

```
wget https://storage.googleapis.com/dart-archive/channels/stable/release/1.24.3/sdk/dartsdk-macos-x64-release.zip
```

> 这个下载地址需要 FQ ，可以到笔者搭建的 Mirrors 站点下载：[https://mirrors.xiaosongfu.com](https://mirrors.xiaosongfu.com/#/dartlang/dartlang)

##### 3.下载完成之后解压

```
unzip dartsdk-macos-x64-release.zip -d ~/develop/dart
```

解压后的文件保存在 `~/develop/dart/dart-sdk` 目录下。

##### 4.将 bin 目前加到系统 PATH 环境变量

```
vi /etc/profile
```

在文件中添加一下内容：

```
export DART="~/develop/dart/dart-sdk"

export PATH=$PATH:$DART/bin
```

##### 5.测试安装是否成功

```
dart --version
```