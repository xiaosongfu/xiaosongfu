---
title: Jenkins & Gradle for CI
date: 2017-02-21 12:15:01
tags: ["jenkins","gradle","ci"]
categories: ["jenkins","gradle"]
---

## 目录
>  
1、简述  
2、适应于本地打包和 Jenkins 打包的签名配置  
3、打包产物 apk 文件的命名    
4、自动化版本号  
5、自动化版本名称  
6、扫描二维码下载 apk  


### 1、简述  
CI、CD 什么的我就不说了，相信没有人比 Google 更了解，我把我看过的好的关于安装配置 Jenkins 的文章都附在了下面：  

>   
[Android Jenkins+Git+Gradle持续集成-实在太详细](http://www.jianshu.com/p/38b2e17ced73#)  
[使用Jenkins搭建iOS/Android持续集成打包平台](http://debugtalk.com/post/iOS-Android-Packing-with-Jenkins/)  
[基于 Jenkins 快速搭建持续集成环境](https://www.ibm.com/developerworks/cn/java/j-lo-jenkins/)  
[基于Gitlab CI搭建持续集成环境](http://www.jianshu.com/p/705428ca1410)  


### 2、适应于本地打包和 Jenkins 打包的签名配置  
#### 2.1 gradle 脚本  

```  
signingConfigs {
    release {
        Properties properties = new Properties()
        if(rootProject.file("local.properties").exists()) {
            properties.load(rootProject.file("local.properties").newDataInputStream())
        }
        storeFile file(properties.get("storeFile") ?: String.valueOf(System.getenv("CI_JENKINS_DEMO_STORE_FILE")))
        storePassword properties.get("storePassword") ?: String.valueOf(System.getenv("CI_JENKINS_DEMO_STORE_PASSWORD"))
        keyAlias properties.get("keyAlias") ?:  String.valueOf(System.getenv("CI_JENKINS_DEMO_KEY_ALIAS"))
        keyPassword properties.get("keyPassword")  ?: String.valueOf(System.getenv("CI_JENKINS_DEMO_KEY_PASSWORD"))
    }
}
```  

脚本的功能是先读取项目根目录下的 local.properties 文件，如果它存在，就将它里面配置的属性读取到 properties 中；然后从 properties 中分别获取 storeFile 等属性，如果 properties 中有相应属性对应的值，则使用其值，如果没有则通过 System.getenv("xx") 获取相应系统环境变量对应的值。  

>  
如果本地有 local.properties 文件，并且里面也配置了 storeFile 等属性，则适合于本地手动打包；  
如果本地没有 local.properties 文件，那么从 properties 中获取到的 storeFile 等属性将为空，转而使用系统配置的环境变量，则适合于 CI 打包。  


Android 项目都会在 .gitignore 文件中配置忽略 local.properties 文件和 jks 签名文件，所以当 CI 服务器从远程仓库垃取到代码后，在项目的根目录下是没有 local.properties 文件的，这时就应该使用在打包服务器上配置的环境变量；而如果是在本地打包，则肯定是有 local.properties 文件的，只需要在里面配置好 storeFile 等属性即可。所以接下来要在本地和 CI 服务器上做相应的配置。  

#### 2.2 配置本地打包  
在本地项目根目录的 local.properties 文件中增加的配置如下：  

```  
# 为本地生成签名 APK 配置签名信息
storeFile=jenkinsdemo.jks
storePassword=1234
keyAlias=jenkinsdemo
keyPassword=1234
```  

从 storeFile 的配置来看，需要将签名文件 jenkinsdemo.jks 放在 module 的根目录下，这个需要当心；而且每次重新检出项目代码到本地开发的时候，如果有在本地打签名包的需求，还需要拷贝 jenkinsdemo.jks 文件和配置好 local.properties 文件。  

#### 2.3 配置 CI 服务器打包  
在 Jenkins 服务器主面板的 系统管理 > 系统设置 > 全局属性 下勾选 Environment variables，并增加环境变量，需要配置4个环境变量，环境变量的键使用 signingConfigs 中指定的，值按照自己的情况来配置即可。如下图所示：

![你看不到我^_^](https://lollipop.xiaosongfu.com/blog/201702/jenkins-env.png)

其中签名文件的路径是需要先将签名文件上传到服务器，然后才能拿到它的绝对路径并配置。

--> 这里实现的“本地打包和 Jenkins 打包的签名配置”只是一种实现方式，欢迎交流其他的实现方式。  

### 3、保存打包产物
打包生成的 apk 文件可能需要复制到其他目录做永久保存，也可能需要上传到第三方应用内侧分发平台；但是如果启用的混淆，那么 mapping 文件则是必须要保存下来的，因为当线上的应用发生 crash 时，为了还原堆栈信息，mapping 映射文件是必备可少的。  
