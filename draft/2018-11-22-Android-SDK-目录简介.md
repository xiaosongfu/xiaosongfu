![](https://lollipop.xiaosongfu.com/blog/201811/androi-sdk-dirs.png) 

![](https://lollipop.xiaosongfu.com/blog/201811/install-androi-sdk.png) 

#### build-tools

SDK Build Tools 作为 Android SDK 一个组件，被用于构建 Android 应用。

从 Android Gradle Plugin 3.0.0 以后，项目会默认自动使用 Android Gradle Plugin 指定的 SDK Build Tools 版本，以前的配置现在已经被废除了，项目中不再需要配置了。

> 以前的配置方法，现在已经不需要配置了：

```
android {
    buildToolsVersion "28.0.3"
    ...
}
```

![](https://lollipop.xiaosongfu.com/blog/201811/androi-sdk-dirs-build-tools.png.png) 

#### tools

如果您不需要 Android Studio，可以下载基本的 Android 命令行工具。您可以使用随附的 sdkmanager 下载其他 SDK 软件包。

里面包含了 sdkmanager，然后可以使用该命令在命令行下载其他 SDK 软件包；与之相对的就是通过安装 Android Studio 的时候直接下载 SDK 软件包或后期通过 Android Studio 的图形化界面下载，而不是命令行。

![](https://lollipop.xiaosongfu.com/blog/201811/androi-sdk-dirs-tools.png) 

#### platform-tools

平台相关的工具，和 Android OS 分离开来更新，因为包含了一些不做 Android 开发也会用到工具，比如 adb 等，比如刷机的时候就会用到 adb、fastboot 等工具。

![](https://lollipop.xiaosongfu.com/blog/201811/androi-sdk-dirs-platform-tools.png) 