---
title: IntelliJ IDEA 打包 Spring Boot 项目为 jar 包
date: 2016-12-15 15:59:10
tags: ["springboot","jar"]
categories: ["springboot"]
---

## 目录
>  
1、配置  
2、打包  


### 1、配置  
将 Spring Boot 项目打成 jar 包十分的简单。首先打开 File > Project Structure ：  

![你看不到我^_^](https://lollipop.xiaosongfu.com/blog/201612/springboot01.jpg)  

然后选中 Artifacts ，再点击 + 号，选择 JAR ，再选择 From modules with dependecies... ：  

![你看不到我^_^](https://lollipop.xiaosongfu.com/blog/201612/springboot02.jpg)  

接着会打开如下界面：  

![你看不到我^_^](https://lollipop.xiaosongfu.com/blog/201612/springboot03.jpg)  

点击图中1所标识的地方，并在弹出的窗口 Select Main Class 里双击你的主类，并点击 OK 。  
接着选中如下图所示的 Build on make 复选框，最后点击底部的 Apply 和 OK。

![你看不到我^_^](https://lollipop.xiaosongfu.com/blog/201612/springboot04.jpg)  

至此，配置就完成了。  

### 2、打包
点击侧边栏的 Maven Projects ，展开 Maven Projects 面板：  

![你看不到我^_^](https://lollipop.xiaosongfu.com/blog/201612/springboot05.jpg)

双击 Lifecycle 下的 package ，接着控制台会有信息打印，如果项目没有错误，打包成功后会在项目的 target  目录下生成 '项目名-版本号.jar' 文件，他就是你要的东西了。  

![你看不到我^_^](https://lollipop.xiaosongfu.com/blog/201612/springboot06.jpg)
