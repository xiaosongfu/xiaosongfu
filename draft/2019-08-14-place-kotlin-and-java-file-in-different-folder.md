
在不同文件夹保存 kotlin 和 java 文件


---

参考：https://developer.android.google.cn/kotlin/add-kotlin

By default, new Kotlin files are saved in `src/main/java/`, which makes it easy to see both Kotlin and Java files in one location. If you'd prefer to separate your Kotlin files from your Java files, you can put Kotlin files under `src/main/kotlin/` instead. If you do this, then you also need to include this directory in your `sourceSets` configuration, as shown below:

```
android {
   sourceSets {
       main.java.srcDirs += 'src/main/kotlin'
   }
}
```
