---
title: Macaca + uirecorder 实现 Android 自动化测试
date: 2017-01-10 13:23:14
tags: ["macaca","ui test"]
categories: ["macaca"]
---

## 目录
>  
1、安装 Macaca    
2、安装 uirecorder    
3、测试  


Macaca 和 uirecorder 都是基于 Node.js 开发的，所以安装它们之前需要先安装 Node 环境，并且建议将 npm 源切换到国内，比如淘宝 npm 源。后文都是使用淘宝的 npm 源。  
Macaca GitHub：[alibaba macaca](https://github.com/alibaba/macaca)  
Macaca Home：[macacajs](https://macacajs.com/zh/)  
uirecorder GitHub：[alibaba uirecorder](https://github.com/alibaba/uirecorder)  

### 1、安装 Macaca  
先安装 Macaca，为了实现 Android 的自动化测试，还需要安装 Macaca 的 Android 驱动，命令：  

```
cnpm install -g macaca-cli  
cnpm install -g macaca-android  
```

Macaca 还有其他平台的驱动，如 iOS 的驱动：npm install -g macaca-ios ，Chrome 的驱动：npm install -g macaca-chrome  

安装完成之后检查一下是否安装成功，运行命令：  

```
macaca doctor
```

运行结果如下，即表示安装成功。注意：环境变量的值跟具体的设置有关  

```
C:\Users\xiaos>macaca doctor

  macaca-doctor version: 1.0.28


  Node.js checklist:

  node env: C:\Program Files\nodejs\node.exe
  node version: v4.6.0

  Android checklist:

  JAVA version is `1.8.0_101`
  JAVA_HOME is set to `C:\Program Files\Java\jdk1.8.0_101`
  ANDROID_HOME is set to `C:\work\android\sdk`
  Platforms is set to `C:\work\android\sdk\platforms\android-24`
  Android tools is set to `C:\work\android\sdk\tools\android.bat`
  ADB tool is set to `C:\work\android\sdk\platform-tools\adb.exe`
  ANT_HOME is set to `C:\work\apache-ant-1.9.7`

  Installed driver list:

  android: 1.1.15
```

### 2、安装 uirecorder  
运行命令安装 uirecorder 和 mocha ：  

```
npm install -g uirecorder mocha
```

安装完成之后运行 uirecorder --version 命令查看 uirecorder 版本信息，如下即表示安装成功：

```
C:\Users\xiaos>uirecorder --version
    __  ______   ____                           __
   / / / /  _/  / __ \___  _________  _________/ /__  _____
  / / / // /   / /_/ / _ \/ ___/ __ \/ ___/ __  / _ \/ ___/
 / /_/ // /   / _, _/  __/ /__/ /_/ / /  / /_/ /  __/ /
 \____/___/  /_/ |_|\___/\___/\____/_/   \__,_/\___/_/    v2.3.23

------------------------------------------------------------------

2.3.23

C:\Users\xiaos>
```

### 3、  测试  
#### 3.1 初始化测试项目  

```
uirecorder init --mobile
npm install
```

使用 npm install 安装 uirecorder 项目的依赖模块。  

#### 3.2 启动 Macaca 服务  

```
macaca server --port 4444
```

#### 3.3 启动 uirecorder 录制测试脚本  

```
uirecorder start --mobile
```

#### 3.4 使用录制的测试脚本进行测试  


```
mocha test.spec.js
```
