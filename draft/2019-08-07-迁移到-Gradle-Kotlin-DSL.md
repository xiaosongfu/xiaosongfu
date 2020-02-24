

参考：[Migrating build logic from Groovy to Kotlin](https://guides.gradle.org/migrating-build-logic-from-groovy-to-kotlin/)

https://proandroiddev.com/takeaways-from-migrating-a-complex-android-project-to-gradle-kotlin-dsl-eaeb5ccd8c84

https://jonnyzzz.com/blog/2019/03/04/gradle-kotlin-migration-1/
https://jonnyzzz.com/blog/2019/04/02/gradle-kotlin-migration-2/
https://jonnyzzz.com/blog/2019/05/20/gradle-kotlin-migration-3/
https://jonnyzzz.com/blog/2019/06/25/gradle-kotlin-migration-4/

---

### 1. 迁移前准备

* Groovy 的字符串可以使用 '' 和 ""，而 Kotlin 只能使用 ""
* Groovy 在调用函数时可以省略括号，而 Kotlin 不能省略
* Groovy 允许赋值的时候省略 =，而 Kotlin 不能省略

如：

```
group 'com.acme'
dependencies {
    implementation 'com.acme:example:1.0'
}
```

将 '' 换成 "" ：

```
group "com.acme'"
dependencies {
    implementation "com.acme:example:1.0"
}
```

再补上省略的 = 和括号：

```
group = "com.acme'"
dependencies {
    implementation("com.acme:example:1.0")
}
```

修改完成后是有效的 Groovy 脚本，语义更明确并且更接近 Kotlin 语法。

### 2. 脚本文件命名

* Groovy DSL 脚本文件使用 `.gradle` 扩展名
* Kotlin DSL 脚本文件使用 `.gradle.kts` 扩展名

所以，只需要将 `build.gradle` 改为 `build.gradle.kts`，将 `settings.gradle` 改为 `settings.gradle.kts`。

> 当然，在多 module 的项目中，Groovy DSL 脚本文件(build.gradle) 和 Kotlin DSL 脚本文件(build.gradle.kts) 可以混合使用。

### 3. 应用插件

和 Groovy DSL 一样，Kotlin DSL 也有两种方式来配置插件：

* 声明式，使用 `plugins {}` 块
* 命令式，使用 `apply(...)` 方法

在 Kotlin 中推荐使用声明式 `plugins {}` 块，如：

```
plugins {
    id("java")
    id("jacoco")
}
```

Kotlin DSL 为所有 **Gradle 核心插件** 提供了属性扩展，可简化声明，如：

```
plugins {
    java
    jacoco
}
```

具体都有哪些可以在这里找到：`BuiltinPluginIdExtensions.kt`，比如：

* java : org.gradle.java
* application : org.gradle.application
* java-gradle-plugin : org.gradle.java-gradle-plugin

```
inline val org.gradle.plugin.use.PluginDependenciesSpec.`application`: org.gradle.plugin.use.PluginDependencySpec
    get() = id("org.gradle.application")
```

### 4. 配置插件

许多插件都带有**扩展**来配置它们。如果使用声明性 `plugins {}` 块应用这些插件，则可以使用 Kotlin 扩展函数来配置其扩展，与 Groovy 中的方式相同。

如：

```
plugins {
    jacoco
}

jacoco {
    toolVersion = "0.8.1
}
```

相反，在 Kotlin DSL 中，如果您使用命令式 `apply()` 函数来应用插件，那么您将不得不使用 `configure<T>()` 函数来配置该插件。

如：

```
apply(plugin = "checkstyle")

configure<CheckstyleExtension> {
    maxErrors = 10
}
```

> Groovy DSL 中 `plugins {}` 和 `apply()` 应用的插件，都使用同样的方法配置插件。

### 5. xx

Gradle 添加了 kotlin() 辅助函数来简化构建脚本：

```
plugins { 
  kotlin("jvm") version "1.3.21"
}


dependencies {
  implementation(kotlin("stdlib"))
  implementation(kotlin("reflect"))
}
```

### 6. task

```
tasks.register<Delete>("removeLocalSzJarsCache") {

}
```

---

为了保证工程可以使用这个脚本，需要在 `settings.gradle.kts` 中添加一行代码，让 Gradle 知道使用 `build.gradle.kts` 脚本来执行构建：

```
rootProject.buildFileName = "build.gradle.kts"
```

---

```
buildTypes {
    release {
        minifyEnabled false
        proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
    }
}

buildTypes {
    getByName("release") {
        isMinifyEnabled = false
        proguardFiles(getDefaultProguardFile("proguard-android.txt"), "proguard-rules.pro")
    }
}
```

```
implementation(fileTree(mapOf("dir" to "libs", "include" to listOf("*.jar"))))
```

---

完整的参考：https://github.com/DroidKaigi/conference-app-2018/blob/master/app/build.gradle.kts