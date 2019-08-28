
参考文章：

* [Android Studio - 从Maven Central迁移到JCenter（发表于 2015年2月9日由JBaruch）](https://blog.bintray.com/2015/02/09/android-studio-migration-from-maven-central-to-jcenter/)

---

有许多将Maven Central替换成jcenter的理由，下面是几个主要的原因。
* jcenter通过CDN发送library，开发者可以享受到更快的下载体验。
* jcenter是全世界最大的Java仓库，因此在Maven Central 上有的，在jcenter上也极有可能有。换句话说jcenter是Maven Central的超集。
* 上传library到仓库很简单，不需要像在 Maven Central上做很多复杂的事情


