---
title: 配置属于你自己的 AndroidStudio
date: 2016-12-05 11:13:47
tags: ["android","android studio"]
categories: ["android"]
---

> 持续更新...  

## 目录
>  
1、智能提示大小写不敏感  
2、显示空格符，并用4个空格代替tab键
3、自定义自动生成的文件头注释


### 1、智能提示大小写不敏感
Android Studio的智能提示默认是大小写敏感的，可以设置为大小写不敏感，设置方法如图：

![case-sensitive.png](https://lollipop.xiaosongfu.com/blog/201612/case-sensitive.png)

### 2、显示空格符，并用4个空格代替tab键
习惯在文件中显示空格符，并且使用4个空格代替tab键，输入一个tab时会自动替换为4个空格，这纯属个人习惯，配置方法如下：
①先配置显示空格符，如图：


![whitespace](https://lollipop.xiaosongfu.com/blog/201612/whitespace.png)

②配置不使用tab符号

![use-tab](https://lollipop.xiaosongfu.com/blog/201612/use-tab.png)

### 3、自定义自动生成的文件头注释
新建一个java文件时会自动生成类注释，但默认的注释格式可能不是我们想要的，因此我们可以自定义类注释的格式，方法如下图：

![file-header](https://lollipop.xiaosongfu.com/blog/201612/file-header.png)

可以使用${YEAR}、${MONTH}等变量。
