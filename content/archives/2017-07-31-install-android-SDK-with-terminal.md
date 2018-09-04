---
title: 使用命令行安装 Android SDK
date: 2017-07-31 13:40:47
tags: ["android","android sdk"]
categories: ["android"]
---

## 目录
>  
1、简述  
2、下载基本的 Android 命令行工具  
3、android & sdkmanager 命令的使用     


### 1、简述  
通常时候我们需要在 Linux 服务器上搭建 Android 打包环境，目的是为了使用 Jenkins 来进行 CI 或 CD ，但是差不多所有的 Linux 服务器都是 mini 化安装的，即是最小化安装，是没有桌面环境的，意味着我们安装软件都得通过命令行安装。当然你也可以把你本地 Linux 环境的 Android SKD 打包后 scp 到服务器，但是没必要这么麻烦，直接通过命令行安装也是十分简单的。  

### 2、下载基本的 Android 命令行工具  
先来个链接：[https://developer.android.com/studio/index.html](https://developer.android.com/studio/index.html)  ,打开后滑到最底部：  
![你看不到我^_^](https://lollipop.xiaosongfu.com/blog/201707/androidSDK-1.png)  
下载完成后的文件为：sdk-tools-linux-3859397.zip，解压它：  
![你看不到我^_^](https://lollipop.xiaosongfu.com/blog/201707/androidSDK-2.png)  

### 3、android & sdkmanager 命令的使用    
### 3.1 安装 platform-tools  
执行命令安装  platform-tools  
```  
tools/android update sdk --no-ui
```  
运行该命令后，首先需要同意协议，然后就会自动开始下载 platform-tools ,下载完成后如下：  
![你看不到我^_^](https://lollipop.xiaosongfu.com/blog/201707/androidSDK-3.png)  

### 3.2 查看当前安装了哪些 Android SDK 组件  
执行命令:  
```  
tools/bin/sdkmanager --list
```  
可以查看当前已经安装了哪些 SDK 组件，运行如下：
```    
[root@maven androidsdk]# tools/bin/sdkmanager --list
Warning: File /root/.android/repositories.cfg could not be loaded.
Installed packages:
  Path           | Version | Description                | Location       
  -------        | ------- | -------                    | -------        
  emulator       | 26.1.4  | Android Emulator           | emulator/      
  patcher;v4     | 1       | SDK Patch Applier v4       | patcher/v4/    
  platform-tools | 26.0.0  | Android SDK Platform-Tools | platform-tools/
  tools          | 26.0.2  | Android SDK Tools          | tools/         

Available Packages:
  Path                              | Version      | Description                      
  -------                           | -------      | -------                          
  add-ons;addon-g..._apis-google-15 | 3            | Google APIs                      
  add-ons;addon-g..._apis-google-16 | 4            | Google APIs                      
  add-ons;addon-g..._apis-google-17 | 4            | Google APIs                      
  add-ons;addon-g..._apis-google-18 | 4            | Google APIs                      
  add-ons;addon-g..._apis-google-19 | 20           | Google APIs                      
  add-ons;addon-g..._apis-google-21 | 1            | Google APIs                      
  add-ons;addon-g..._apis-google-22 | 1            | Google APIs                      
  add-ons;addon-g..._apis-google-23 | 1            | Google APIs                      
  add-ons;addon-g..._apis-google-24 | 1            | Google APIs                      
  add-ons;addon-g...e_gdk-google-19 | 11           | Glass Development Kit Preview    
  build-tools;19.1.0                | 19.1.0       | Android SDK Build-Tools 19.1     
  build-tools;20.0.0                | 20.0.0       | Android SDK Build-Tools 20       
  build-tools;21.1.2                | 21.1.2       | Android SDK Build-Tools 21.1.2   
  build-tools;22.0.1                | 22.0.1       | Android SDK Build-Tools 22.0.1   
  build-tools;23.0.1                | 23.0.1       | Android SDK Build-Tools 23.0.1   
  build-tools;23.0.2                | 23.0.2       | Android SDK Build-Tools 23.0.2   
  build-tools;23.0.3                | 23.0.3       | Android SDK Build-Tools 23.0.3   
  build-tools;24.0.0                | 24.0.0       | Android SDK Build-Tools 24       
  build-tools;24.0.1                | 24.0.1       | Android SDK Build-Tools 24.0.1   
  build-tools;24.0.2                | 24.0.2       | Android SDK Build-Tools 24.0.2   
  build-tools;24.0.3                | 24.0.3       | Android SDK Build-Tools 24.0.3   
  build-tools;25.0.0                | 25.0.0       | Android SDK Build-Tools 25       
  build-tools;25.0.1                | 25.0.1       | Android SDK Build-Tools 25.0.1   
  build-tools;25.0.2                | 25.0.2       | Android SDK Build-Tools 25.0.2   
  build-tools;25.0.3                | 25.0.3       | Android SDK Build-Tools 25.0.3   
  build-tools;26.0.0                | 26.0.0       | Android SDK Build-Tools 26       
  build-tools;26.0.1                | 26.0.1       | Android SDK Build-Tools 26.0.1   
  cmake;3.6.4111459                 | 3.6.4111459  | CMake 3.6.4111459                
  docs                              | 1            | Documentation for Android SDK    
  emulator                          | 26.1.4       | Android Emulator                 
  extras;android;gapid;1            | 1.0.3        | GPU Debugging tools              
  extras;android;gapid;3            | 3.1.0        | GPU Debugging tools              
  extras;android;m2repository       | 47.0.0       | Android Support Repository       
  extras;google;auto                | 1.1          | Android Auto Desktop Head Unit...
  extras;google;g...e_play_services | 44           | Google Play services             
  extras;google;instantapps         | 1.0.0        | Instant Apps Development SDK     
  extras;google;m2repository        | 58           | Google Repository                
  extras;google;m...t_apk_expansion | 1            | Google Play APK Expansion library
  extras;google;market_licensing    | 1            | Google Play Licensing Library    
  extras;google;play_billing        | 5            | Google Play Billing Library      
  extras;google;simulators          | 1            | Android Auto API Simulators      
  extras;google;webdriver           | 2            | Google Web Driver                
  extras;m2reposi...ut-solver;1.0.0 | 1            | Solver for ConstraintLayout 1.0.0
  extras;m2reposi...er;1.0.0-alpha2 | 1            | com.android.support.constraint...
  extras;m2reposi...er;1.0.0-alpha3 | 1            | com.android.support.constraint...
  extras;m2reposi...er;1.0.0-alpha4 | 1            | com.android.support.constraint...
  extras;m2reposi...er;1.0.0-alpha5 | 1            | Solver for ConstraintLayout 1....
  extras;m2reposi...er;1.0.0-alpha6 | 1            | Solver for ConstraintLayout 1....
  extras;m2reposi...er;1.0.0-alpha7 | 1            | Solver for ConstraintLayout 1....
  extras;m2reposi...er;1.0.0-alpha8 | 1            | Solver for ConstraintLayout 1....
  extras;m2reposi...er;1.0.0-alpha9 | 1            | Solver for ConstraintLayout 1....
  extras;m2reposi...ver;1.0.0-beta1 | 1            | Solver for ConstraintLayout 1....
  extras;m2reposi...ver;1.0.0-beta2 | 1            | Solver for ConstraintLayout 1....
  extras;m2reposi...ver;1.0.0-beta3 | 1            | Solver for ConstraintLayout 1....
  extras;m2reposi...ver;1.0.0-beta4 | 1            | Solver for ConstraintLayout 1....
  extras;m2reposi...ver;1.0.0-beta5 | 1            | Solver for ConstraintLayout 1....
  extras;m2reposi...ut-solver;1.0.1 | 1            | Solver for ConstraintLayout 1.0.1
  extras;m2reposi...ut-solver;1.0.2 | 1            | Solver for ConstraintLayout 1.0.2
  extras;m2reposi...nt-layout;1.0.0 | 1            | ConstraintLayout for Android 1...
  extras;m2reposi...ut;1.0.0-alpha2 | 1            | com.android.support.constraint...
  extras;m2reposi...ut;1.0.0-alpha3 | 1            | com.android.support.constraint...
  extras;m2reposi...ut;1.0.0-alpha4 | 1            | com.android.support.constraint...
  extras;m2reposi...ut;1.0.0-alpha5 | 1            | ConstraintLayout for Android 1...
  extras;m2reposi...ut;1.0.0-alpha6 | 1            | ConstraintLayout for Android 1...
  extras;m2reposi...ut;1.0.0-alpha7 | 1            | ConstraintLayout for Android 1...
  extras;m2reposi...ut;1.0.0-alpha8 | 1            | ConstraintLayout for Android 1...
  extras;m2reposi...ut;1.0.0-alpha9 | 1            | ConstraintLayout for Android 1...
  extras;m2reposi...out;1.0.0-beta1 | 1            | ConstraintLayout for Android 1...
  extras;m2reposi...out;1.0.0-beta2 | 1            | ConstraintLayout for Android 1...
  extras;m2reposi...out;1.0.0-beta3 | 1            | ConstraintLayout for Android 1...
  extras;m2reposi...out;1.0.0-beta4 | 1            | ConstraintLayout for Android 1...
  extras;m2reposi...out;1.0.0-beta5 | 1            | ConstraintLayout for Android 1...
  extras;m2reposi...nt-layout;1.0.1 | 1            | ConstraintLayout for Android 1...
  extras;m2reposi...nt-layout;1.0.2 | 1            | ConstraintLayout for Android 1...
  lldb;2.0                          | 2.0.2558144  | LLDB 2.0                         
  lldb;2.1                          | 2.1.2852477  | LLDB 2.1                         
  lldb;2.2                          | 2.2.3271982  | LLDB 2.2                         
  lldb;2.3                          | 2.3.3614996  | LLDB 2.3                         
  ndk-bundle                        | 15.2.4203891 | NDK                              
  patcher;v4                        | 1            | SDK Patch Applier v4             
  platform-tools                    | 26.0.0       | Android SDK Platform-Tools       
  platforms;android-10              | 2            | Android SDK Platform 10          
  platforms;android-11              | 2            | Android SDK Platform 11          
  platforms;android-12              | 3            | Android SDK Platform 12          
  platforms;android-13              | 1            | Android SDK Platform 13          
  platforms;android-14              | 4            | Android SDK Platform 14          
  platforms;android-15              | 5            | Android SDK Platform 15          
  platforms;android-16              | 5            | Android SDK Platform 16          
  platforms;android-17              | 3            | Android SDK Platform 17          
  platforms;android-18              | 3            | Android SDK Platform 18          
  platforms;android-19              | 4            | Android SDK Platform 19          
  platforms;android-20              | 2            | Android SDK Platform 20          
  platforms;android-21              | 2            | Android SDK Platform 21          
  platforms;android-22              | 2            | Android SDK Platform 22          
  platforms;android-23              | 3            | Android SDK Platform 23          
  platforms;android-24              | 2            | Android SDK Platform 24          
  platforms;android-25              | 3            | Android SDK Platform 25          
  platforms;android-26              | 2            | Android SDK Platform 26          
  platforms;android-7               | 3            | Android SDK Platform 7           
  platforms;android-8               | 3            | Android SDK Platform 8           
  platforms;android-9               | 2            | Android SDK Platform 9           
  sources;android-15                | 2            | Sources for Android 15           
  sources;android-16                | 2            | Sources for Android 16           
  sources;android-17                | 1            | Sources for Android 17           
  sources;android-18                | 1            | Sources for Android 18           
  sources;android-19                | 2            | Sources for Android 19           
  sources;android-20                | 1            | Sources for Android 20           
  sources;android-21                | 1            | Sources for Android 21           
  sources;android-22                | 1            | Sources for Android 22           
  sources;android-23                | 1            | Sources for Android 23           
  sources;android-24                | 1            | Sources for Android 24           
  sources;android-25                | 1            | Sources for Android 25           
  system-images;a...ult;armeabi-v7a | 4            | ARM EABI v7a System Image        
  system-images;a...-10;default;x86 | 4            | Intel x86 Atom System Image      
  system-images;a...pis;armeabi-v7a | 5            | Google APIs ARM EABI v7a Syste...
  system-images;a...google_apis;x86 | 5            | Google APIs Intel x86 Atom Sys...
  system-images;a...ult;armeabi-v7a | 2            | ARM EABI v7a System Image        
  system-images;a...ult;armeabi-v7a | 4            | ARM EABI v7a System Image        
  system-images;a...15;default;mips | 1            | MIPS System Image                
  system-images;a...-15;default;x86 | 4            | Intel x86 Atom System Image      
  system-images;a...pis;armeabi-v7a | 5            | Google APIs ARM EABI v7a Syste...
  system-images;a...google_apis;x86 | 5            | Google APIs Intel x86 Atom Sys...
  system-images;a...ult;armeabi-v7a | 4            | ARM EABI v7a System Image        
  system-images;a...16;default;mips | 1            | MIPS System Image                
  system-images;a...-16;default;x86 | 5            | Intel x86 Atom System Image      
  system-images;a...pis;armeabi-v7a | 5            | Google APIs ARM EABI v7a Syste...
  system-images;a...google_apis;x86 | 5            | Google APIs Intel x86 Atom Sys...
  system-images;a...ult;armeabi-v7a | 5            | ARM EABI v7a System Image        
  system-images;a...17;default;mips | 1            | MIPS System Image                
  system-images;a...-17;default;x86 | 3            | Intel x86 Atom System Image      
  system-images;a...pis;armeabi-v7a | 5            | Google APIs ARM EABI v7a Syste...
  system-images;a...google_apis;x86 | 5            | Google APIs Intel x86 Atom Sys...
  system-images;a...ult;armeabi-v7a | 4            | ARM EABI v7a System Image        
  system-images;a...-18;default;x86 | 3            | Intel x86 Atom System Image      
  system-images;a...pis;armeabi-v7a | 5            | Google APIs ARM EABI v7a Syste...
  system-images;a...google_apis;x86 | 5            | Google APIs Intel x86 Atom Sys...
  system-images;a...ult;armeabi-v7a | 5            | ARM EABI v7a System Image        
  system-images;a...-19;default;x86 | 6            | Intel x86 Atom System Image      
  system-images;a...pis;armeabi-v7a | 30           | Google APIs ARM EABI v7a Syste...
  system-images;a...google_apis;x86 | 30           | Google APIs Intel x86 Atom Sys...
  system-images;a...-tv;armeabi-v7a | 3            | Android TV ARM EABI v7a System...
  system-images;a...;android-tv;x86 | 3            | Android TV Intel x86 Atom Syst...
  system-images;a...ult;armeabi-v7a | 4            | ARM EABI v7a System Image        
  system-images;a...-21;default;x86 | 5            | Intel x86 Atom System Image      
  system-images;a...;default;x86_64 | 5            | Intel x86 Atom_64 System Image   
  system-images;a...pis;armeabi-v7a | 22           | Google APIs ARM EABI v7a Syste...
  system-images;a...google_apis;x86 | 22           | Google APIs Intel x86 Atom Sys...
  system-images;a...gle_apis;x86_64 | 22           | Google APIs Intel x86 Atom_64 ...
  system-images;a...-tv;armeabi-v7a | 1            | Android TV ARM EABI v7a System...
  system-images;a...;android-tv;x86 | 3            | Android TV Intel x86 Atom Syst...
  system-images;a...ult;armeabi-v7a | 2            | ARM EABI v7a System Image        
  system-images;a...-22;default;x86 | 6            | Intel x86 Atom System Image      
  system-images;a...;default;x86_64 | 6            | Intel x86 Atom_64 System Image   
  system-images;a...pis;armeabi-v7a | 16           | Google APIs ARM EABI v7a Syste...
  system-images;a...google_apis;x86 | 16           | Google APIs Intel x86 Atom Sys...
  system-images;a...gle_apis;x86_64 | 16           | Google APIs Intel x86 Atom_64 ...
  system-images;a...-tv;armeabi-v7a | 11           | Android TV ARM EABI v7a System...
  system-images;a...;android-tv;x86 | 11           | Android TV Intel x86 Atom Syst...
  system-images;a...ear;armeabi-v7a | 6            | Android Wear ARM EABI v7a Syst...
  system-images;a...ndroid-wear;x86 | 6            | Android Wear Intel x86 Atom Sy...
  system-images;a...-23;default;x86 | 10           | Intel x86 Atom System Image      
  system-images;a...;default;x86_64 | 10           | Intel x86 Atom_64 System Image   
  system-images;a...pis;armeabi-v7a | 23           | Google APIs ARM EABI v7a Syste...
  system-images;a...google_apis;x86 | 23           | Google APIs Intel x86 Atom Sys...
  system-images;a...gle_apis;x86_64 | 23           | Google APIs Intel x86 Atom_64 ...
  system-images;a...;android-tv;x86 | 12           | Android TV Intel x86 Atom Syst...
  system-images;a...fault;arm64-v8a | 7            | ARM 64 v8a System Image          
  system-images;a...ult;armeabi-v7a | 7            | ARM EABI v7a System Image        
  system-images;a...-24;default;x86 | 8            | Intel x86 Atom System Image      
  system-images;a...;default;x86_64 | 8            | Intel x86 Atom_64 System Image   
  system-images;a..._apis;arm64-v8a | 16           | Google APIs ARM 64 v8a System ...
  system-images;a...pis;armeabi-v7a | 16           | Google APIs ARM EABI v7a Syste...
  system-images;a...google_apis;x86 | 16           | Google APIs Intel x86 Atom Sys...
  system-images;a...gle_apis;x86_64 | 16           | Google APIs Intel x86 Atom_64 ...
  system-images;a...s_playstore;x86 | 16           | Google Play Intel x86 Atom Sys...
  system-images;a...;android-tv;x86 | 6            | Android TV Intel x86 Atom Syst...
  system-images;a...ear;armeabi-v7a | 3            | Android Wear ARM EABI v7a Syst...
  system-images;a...ndroid-wear;x86 | 3            | Android Wear Intel x86 Atom Sy...
  system-images;a..._apis;arm64-v8a | 8            | Google APIs ARM 64 v8a System ...
  system-images;a...pis;armeabi-v7a | 8            | Google APIs ARM EABI v7a Syste...
  system-images;a...google_apis;x86 | 8            | Google APIs Intel x86 Atom Sys...
  system-images;a...gle_apis;x86_64 | 8            | Google APIs Intel x86 Atom_64 ...
  system-images;a...;android-tv;x86 | 4            | Android TV Intel x86 Atom Syst...
  system-images;a...ndroid-wear;x86 | 1            | Android Wear Intel x86 Atom Sy...
  system-images;a...google_apis;x86 | 5            | Google APIs Intel x86 Atom Sys...
  system-images;a...s_playstore;x86 | 5            | Google Play Intel x86 Atom Sys...
  tools                             | 26.0.2       | Android SDK Tools                
done
[root@maven androidsdk]#  
```    

### 3.3  查看用哪些 Android SDK 组件可以安装
可以使用如下两个命令来查看有哪些 Android SDK 组件可以安装：
```  
android list sdk --all
android list sdk --all --extended
```  
他们的区别只是显示的结果不一样（看参数就知道的），以 android list sdk --all 为例，运行结果如下：  
``` 
 
``` 
### 3.4 安装 Android SDK 组件
