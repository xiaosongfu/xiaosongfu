---
title: "Android Studio 彻底清除代理设置"
date: 2019-02-20T17:31:39+08:00
tags: []
categories: []
---

在 Android Studio 的 `Setting -> Appearance & Behavior -> System Settings -> HttpProxy` 设置里面选择 `Manual proxy configuration` 并设置好代理后，会在项目根目录下的 `gradle.properties` 文件中添加以下配置：

![](https://lollipop.xiaosongfu.com/blog/201902/gradle-properties-project.png)

但是当在设置里面关闭了代理，选择 `No proxy`，并且项目根目录下的 `gradle.properties` 文件中的代理配置也被删除后，会出现每次 build 都需要代理，都要把代理开着才可以 build 成功，

怎么解决呢？

解决方案是需要清除用户主目录下的 `.gradle/gradle.properties` 文件里面配置的代理，如下图：

```
$ cd ~/.gradle/

fuxiaosongdeMac-mini:.gradle fuxiaosong$ ls -la
total 8
drwxr-xr-x    9 fuxiaosong  staff   288 Feb 20 17:20 .
drwxr-xr-x+ 126 fuxiaosong  staff  4032 Feb 20 17:20 ..
drwxr-xr-x   16 fuxiaosong  staff   512 Jan 28 13:54 caches
drwxr-xr-x   17 fuxiaosong  staff   544 Nov 28 15:32 daemon
-rw-r--r--    1 fuxiaosong  staff   719 Feb 20 17:20 gradle.properties
drwxr-xr-x    6 fuxiaosong  staff   192 Nov 22  2017 native
drwxr-xr-x    4 fuxiaosong  staff   128 Nov 22 16:01 notifications
drwxr-xr-x    2 fuxiaosong  staff    64 Nov 22  2017 workers
drwxr-xr-x    4 fuxiaosong  staff   128 Aug 25  2017 wrapper

$ vi gradle.properties 
```

![](https://lollipop.xiaosongfu.com/blog/201902/gradle-properties-global.png)

需要将代理部分删除掉！！！