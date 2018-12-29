Android Support Library 已经走过7年了，它为新的框架 API 提供向后兼容。AndroidX 会通过提供相同的功能和更多新增的库来完全替代 Support Library。之前的 support 库太过于混乱，AndroidX 将所有的包都迁移到了 androix.* 包下面，并且严格遵循语义化版本号规则（从1.0.0开始发布）。Support Library Revision 28.0.0 在 2018年9月21号正式发布，宣布了这将会是最后一个 support 包，并鼓励开发者迁移到 AndroidX。

### 迁移步骤

在 `gradle.properties` 中添加如下配置：

```
# Migrating to AndroidX
# For more information,please visit
# https://developer.android.google.cn/jetpack/androidx/
android.useAndroidX=true
android.enableJetifier=true
```

然后在 Android Studio 中点击 Refactor 菜单，选择 Migrate to AndroidX 即可。

![](https://lollipop.xiaosongfu.com/blog/201811/migrate-to-androidx.png) 

这会影响到：
1. build.gradle 文件中的依赖路径，如：`androidx.test:runner:1.1.0`
2. 代码文件中类的依赖路径，如：`androidx.recyclerview.widget.RecyclerView`

迁移之后的对比：

> support-library
 
![](https://lollipop.xiaosongfu.com/blog/201811/support-library-1.png) 

![](https://lollipop.xiaosongfu.com/blog/201811/support-library-2.png) 

> androidx

![](https://lollipop.xiaosongfu.com/blog/201811/androidx-1.png) 

![](https://lollipop.xiaosongfu.com/blog/201811/androidx-2.png) 

### 对比示例

android.support.v7.app.AppCompatActivity ==> androidx.appcompat.app.AppCompatActivity
android.support.v7.widget.CardView ==> androidx.cardview.widget.CardView
android.support.v7.widget.RecyclerView ==> androidx.recyclerview.widget.RecyclerView

> 迁移前

```
package com.xiaosongfu.talkme

import android.support.v7.app.AppCompatActivity
import android.os.Bundle
import android.support.v7.widget.CardView
import android.support.v7.widget.RecyclerView

class MainActivity : AppCompatActivity() {
    var recyclerView: RecyclerView? = null
    var cardView: CardView? = null

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
    }
}
```

> 迁移后

```
package com.xiaosongfu.talkme

import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import androidx.cardview.widget.CardView
import androidx.recyclerview.widget.RecyclerView

class MainActivity : AppCompatActivity() {
    var recyclerView: RecyclerView? = null
    var cardView: CardView? = null

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
    }
}
```
