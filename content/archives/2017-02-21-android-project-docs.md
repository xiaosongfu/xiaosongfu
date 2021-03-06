---
title:  Android 项目必备文档
date: 2017-02-21 13:37:16
tags: ["android","docs"]
categories: ["android"]
---

## 目录
>  
1、IDE 配置文档  
2、编码规范文档  
3、日志打印规范文档  
4、源代码版本控制文档  
5、第三方服务集成文档  
6、版本号规则文档  
7、发版流程文档  
8、APP 版本更新文档  
9、测试机清单文档  
10、应用模块分工文档  
11、应用页面逻辑流程文档  
12、API 分布文档  
13、库使用手册  


>  
先道个歉：说必备有点不要 face 了，但我又想不到可以用别的什么词来修饰比较合适。  
再说个明：标题说是 Android 项目，其实 iOS 项目也是适应的。

* 部分内容参考了包建强老师的《APP 研发录》这本书。  

下面开始正文。  
作为程序猿，个人的能力其实不应该局限于编码能力，文档的写作能力也尤为重要，只会码代码不会写文档的程序猿都不是好程序猿。  
文档齐全的项目对项目组新人更加的友好，既可提升项目友好度，又可以让新人更快的融入到项目的开发中，尽快开始上手编码。  

我把项目分为应用项目和库项目，库项目除了要包含应用项目所包含的文档外，还应该包含用户友好的 “库使用手册”。1-8为通用共通的应用项目开发手册，9-12为与具体项目相关的应用项目开发手册，13为库项目的库使用手册。  

### 1、IDE 配置文档  
团队开发，不同于单兵作战，统一开发环境十分的重要。我们可能需要统一 IDE 的项目编码和文件编码；还可以统一文件头注释，还有单行字符数量限制等等。让大家的开发环境趋于统一，这样必将减少很多的麻烦。  

### 2、编码规范文档  
编码规范对一个项目的后期维护十分重要，而每一个开发人员可能都有自己根深蒂固的开发习惯，我们应该统一团队的编码规范，让大家都基于同一套规范来编码。阿里巴巴刚刚对外公开的 JAVA 开发手册相信是最佳的规范，好的编码习惯不是一天养成的，加油吧。  

### 3、日志打印规范文档  
在调试模式下哪些信息是需要打印的，日志的级别是什么等等这些都应该在该文档里规范化。比如发起网络请求的请求地址，请求体，服务器原始响应，服务器返回的 JSON 字符串等等这些在调试模式下都应该是要打印出来以便于调试。  

### 4、源代码版本控制文档  
(1) 首先需要提供源代码的仓库地址。  
(2) 如何检出源代码以及检出源代码以后需要做哪些配置才能将项目运行起来。  
(3) 开始开发之前需要做哪些准备，比如是否需要新建新的功能分支，开发人员是否可以自己建包等等。  
(4) 提交代码时的 message 的规范，比如是否需要在 message 中体现本次提交是新增功能，还是 bug 修复，还是别的什么，等等。  

### 5、第三方服务集成文档  
项目在什么时候集成了什么第三方服务，集成的步骤是什么样的，所有的这些都应该以文档的形式被记录下来，这对项目的维护十分的重要，而且当项目换了负责人以后，也能通过该文档清晰的知道项目集成了哪些第三方服务，如果有更换第三方服务的需求，比如把推送服务从极光换成小米或是别的什么，也能够简单快捷不误操作的搞定。说到第三方服务，其实注册第三方服务的时候，应该用公司账号而不应该是项目负责人的私人账号，这样在项目更换负责人的时候就能少掉很多的麻烦。  

### 6、版本号规则文档  
应用发版的时候，版本号建议遵循 * 语义化的版本控制 * 规则，即是：标准的版本号必须采用 X.Y.Z 的格式，其中 X、Y、Z 为非负的整数，且禁止在数字前方加前导零。X 是主版本号、Y 是次版本号、 Z 为修订号。每个元素必须以数值来递增。 具体参见官网：[语义化版本2.0.0](http://semver.org/lang/zh-CN/)  

### 7、发版流程文档  
应用要发版时，需要检查一下签名文件、签名信息是否正确的准备好了，如果还配合着 CI 服务，CI 服务器是否也配置好了。发版之前要检查渠道号是否正确、版本号是否正确、代码是否混淆了、是否还需要加固等等，而且打出来的包还需要测试一下，测试是否能正常升级正常安装，此外测试人员还应该随机抽一个包进行功能点的测试，保证打出来的包没有任何问题。  

### 8、APP 版本更新文档  
APP 每一个版本新增、优化了哪些功能，修复了哪些 bug 都应该被详细的记录下来。在 APP 升级的时候应该将这些信息需要告知用户，让用户可以选择是否更新应用，同时也是对 APP 的成长轨迹有一个详细记录。  
可以参考下图：  

![你看不到我^_^](https://lollipop.xiaosongfu.com/blog/201702/updatedoc.png)   

### 9、测试机清单文档  
APP 研发团队都应该有一份测试机清单，便于线上有类似机型发生 crash 时，可以有机会重现这个问题。  
可以参考下图：  

![你看不到我^_^](https://lollipop.xiaosongfu.com/blog/201702/testphone.png)   

### 10、应用模块分工文档  
将项目按照各个模块分配给指定开发人员，每个模块有主力开发人员和后备开发人员，两者互为备份，分工一旦确定就尽量不要随意调整。  
可以参考下图：  

![你看不到我^_^](https://lollipop.xiaosongfu.com/blog/201702/moduletask.png)   

### 11、应用页面逻辑流程文档  
每一个业务的逻辑都是非常复杂的，当我们想修改页面跳转逻辑及传参时，往往会因为考虑不全面而引发灾难性问题，这就使得我们迫切需要每条业务线的页面逻辑流程图。页面逻辑流程图详细记录了各个页面之间的跳转关系以及参数传递关系，使得在更新功能时有迹可循。  
可以参考下图：

![你看不到我^_^](https://lollipop.xiaosongfu.com/blog/201702/flow.png)  

### 12、API 分布文档  
API 分布文档记录了服务器提供的 API 分布在哪些界面。当服务器更新 API 以后开发人员可以快速找到需要更新的界面，测试人员快速找到接口的更新影响了哪些界面和功能，哪些界面和功能需要进行回归测试。  
可以参考下图：   

![你看不到我^_^](https://lollipop.xiaosongfu.com/blog/201702/api.png)   

### 13、库使用手册  
对于一个库项目，使用手册尤其重要，而且使用方法应该做到尽量的简单，以降低使用门槛。当然库项目要是能开源就更好了，显然这对于公司的一些库项目是不现实的。  
库的使用手册一般都是写在项目的 README.md 文件。
库项目的使用手册应该尽量包含如下内容：  

>  
* 引入库项目的方式，比如是使用 maven 依赖还是 gradle 依赖，是否需要配置特有的仓库地址  
* 引入项目后是否还需要做一些配置，以及应该具体怎么配置，要配置哪些配置项等等  
* 库项目的主要使用方法，尽量介绍得详细而全面一点，便于快速上手使用  
* 库项目的混淆配置方法，要清楚的说明应该如何配置混淆规则  
