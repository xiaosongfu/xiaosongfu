1. 初始化项目
2. 查看可以执行哪些 task

---

## 1. 初始化项目

使用 `gradle init` 命令初始化项目，记得选择 application 类型、kotlin 语言和 kotlin DSL：

```
$ gradle init
Starting a Gradle Daemon (subsequent builds will be faster)

Select type of project to generate:
  1: basic
  2: application
  3: library
  4: Gradle plugin
Enter selection (default: basic) [1..4] 2

Select implementation language:
  1: C++
  2: Groovy
  3: Java
  4: Kotlin
  5: Swift
Enter selection (default: Java) [1..5] 4

Select build script DSL:
  1: Groovy
  2: Kotlin
Enter selection (default: Kotlin) [1..2] 2

Project name (default: gradle-test):
Source package (default: gradle.test): ltd.xsfu.test

BUILD SUCCESSFUL in 38s
2 actionable tasks: 2 executed
```

初始化后项目目录结构如下：

```
tree
.
├── build.gradle.kts
├── gradle
│   └── wrapper
│       ├── gradle-wrapper.jar
│       └── gradle-wrapper.properties
├── gradlew
├── gradlew.bat
├── settings.gradle.kts
└── src
    ├── main
    │   ├── kotlin
    │   │   └── ltd
    │   │       └── xsfu
    │   │           └── test
    │   │               └── App.kt
    │   └── resources
    └── test
        ├── kotlin
        │   └── ltd
        │       └── xsfu
        │           └── test
        │               └── AppTest.kt
        └── resources

15 directories, 8 files
```

> settings.gradle.kts 内容如下：

```
rootProject.name = "gradle-test"
```

> build.gradle.kts 内容如下：

```
plugins {
    // Apply the Kotlin JVM plugin to add support for Kotlin.
    id("org.jetbrains.kotlin.jvm") version "1.3.61"

    // Apply the application plugin to add support for building a CLI application.
    application
}

repositories {
    // Use jcenter for resolving dependencies.
    // You can declare any Maven/Ivy/file repository here.
    jcenter()
}

dependencies {
    // Align versions of all Kotlin components
    implementation(platform("org.jetbrains.kotlin:kotlin-bom"))

    // Use the Kotlin JDK 8 standard library.
    implementation("org.jetbrains.kotlin:kotlin-stdlib-jdk8")

    // Use the Kotlin test library.
    testImplementation("org.jetbrains.kotlin:kotlin-test")

    // Use the Kotlin JUnit integration.
    testImplementation("org.jetbrains.kotlin:kotlin-test-junit")
}

application {
    // Define the main class for the application.
    mainClassName = "ltd.xsfu.test.AppKt"
}
```

如果是只有一个 module 的项目就这样就可以开始写代码了（在 src 目录下写代码），因为 build.gradle.kts 文件里面的配置就是配置了一个 application 类型的 module。

如果是希望配置一个多 module 的项目，这需要做一些修改，首先新建一个 module，比如：apiserver，然后将 build.gradle.kts 文件和 src 目录移动到该 module 里面，然后在项目根目录重新建一个 build.gradle.kts 文件，它的内容如下：

```
plugins {
    // Apply the Kotlin JVM plugin to add support for Kotlin.
    id("org.jetbrains.kotlin.jvm") version "1.3.61"
}

allprojects {
    repositories {
        // google()
        maven {
            url = uri("https://maven.aliyun.com/repository/google")
        }
        // jcenter()
        maven {
            url = uri("https://maven.aliyun.com/repository/jcenter")
        }
        // mavenCentral()
        maven {
            url = uri("https://maven.aliyun.com/repository/central")
        }
        // gradle plugin
        maven {
            url = uri("https://maven.aliyun.com/repository/gradle-plugin")
        }
    }
}
```

还需要修改 settings.gradle.kts 文件，添加如下内容：

```
include(":api", ":model")
```

（按需）再新建其他 module，然后将 src 目录和 build.gradle.kts 文件都复制一份保存到新建的 module 里面即可。

如果希望将一个库类型的 module，则可以通过 `gradle init` 命令选择 library 类型来获得一个 library 类型 module 的 build.gradle.kts 文件，它的内容如下：

```
plugins {
    // Apply the Kotlin JVM plugin to add support for Kotlin.
    id("org.jetbrains.kotlin.jvm") version "1.3.41"

    // Apply the java-library plugin for API and implementation separation.
    `java-library`
}

repositories {
    // Use jcenter for resolving dependencies.
    // You can declare any Maven/Ivy/file repository here.
    jcenter()
}

dependencies {
    // Align versions of all Kotlin components
    implementation(platform("org.jetbrains.kotlin:kotlin-bom"))

    // Use the Kotlin JDK 8 standard library.
    implementation("org.jetbrains.kotlin:kotlin-stdlib-jdk8")

    // Use the Kotlin test library.
    testImplementation("org.jetbrains.kotlin:kotlin-test")

    // Use the Kotlin JUnit integration.
    testImplementation("org.jetbrains.kotlin:kotlin-test-junit")
}
```

## 2. 查看可以执行哪些 task

执行 `gradle tasks` 命令来查看都有哪些可以执行的命令：

```
gradle tasks

> Task :tasks

------------------------------------------------------------
Tasks runnable from root project
------------------------------------------------------------

Application tasks
-----------------
run - Runs this project as a JVM application

Build tasks
-----------
assemble - Assembles the outputs of this project.
build - Assembles and tests this project.
buildDependents - Assembles and tests this project and all projects that depend on it.
buildNeeded - Assembles and tests this project and all projects it depends on.
classes - Assembles main classes.
clean - Deletes the build directory.
jar - Assembles a jar archive containing the main classes.
testClasses - Assembles test classes.

Build Setup tasks
-----------------
init - Initializes a new Gradle build.
wrapper - Generates Gradle wrapper files.

Distribution tasks
------------------
assembleDist - Assembles the main distributions
distTar - Bundles the project as a distribution.
distZip - Bundles the project as a distribution.
installDist - Installs the project as a distribution as-is.

Documentation tasks
-------------------
javadoc - Generates Javadoc API documentation for the main source code.

Help tasks
----------
buildEnvironment - Displays all buildscript dependencies declared in root project 'study-vertx'.
components - Displays the components produced by root project 'study-vertx'. [incubating]
dependencies - Displays all dependencies declared in root project 'study-vertx'.
dependencyInsight - Displays the insight into a specific dependency in root project 'study-vertx'.
dependentComponents - Displays the dependent components of components in root project 'study-vertx'. [incubating]
help - Displays a help message.
kotlinDslAccessorsReport - Prints the Kotlin code for accessing the currently available project extensions and conventions.
model - Displays the configuration model of root project 'study-vertx'. [incubating]
outgoingVariants - Displays the outgoing variants of root project 'study-vertx'.
projects - Displays the sub-projects of root project 'study-vertx'.
properties - Displays the properties of root project 'study-vertx'.
tasks - Displays the tasks runnable from root project 'study-vertx'.

Verification tasks
------------------
check - Runs all checks.
test - Runs the unit tests.

Rules
-----
Pattern: clean<TaskName>: Cleans the output files of a task.
Pattern: build<ConfigurationName>: Assembles the artifacts of a configuration.
Pattern: upload<ConfigurationName>: Assembles and uploads the artifacts belonging to a configuration.

To see all tasks and more detail, run gradle tasks --all

To see more detail about a task, run gradle help --task <task>

BUILD SUCCESSFUL in 25s
1 actionable task: 1 executed
```

* 对于 application 类型的 module 可以执行 `gradle run` 命令启动应用；还可以执行 `gradle distZip` 或 `gradle distTar` 命令将应用打包成一个 zip/tar 包
* 对于 library 类型的 module 可以执行 
