https://www.jianshu.com/p/1850c040271f


```
<提交类型>(影响范围): <主题> 
# 解释为什么要做这些改动
issue #?
```

新功能|bug修复|文档改动|格式化|重构|测试代码

---

```
<type>(<scope>): <subject>
<body>
<footer>
```

type: 值可以有很多
scope: 用来说明此次修改的影响范围 可以随便填写任何东西
subject: 用来简要描述本次改动，概述就好了
body: 具体的修改信息 应该尽量详细
footer: 放置写备注啥的，如果是 bug ，可以把 bug id 放入


新功能
功能优化
修复 bug  
代码重构
代码优化
格式化
发布版本
测试代码
修改文档
需求变更

---

commit message格式说明

Commit message一般包括三部分：Header、Body和Footer。

### Header
type(scope):subject

##### type：用于说明commit的类别，规定为如下几种 
feat：新增功能；
fix：修复bug；
docs：修改文档；
refactor：代码重构，未新增任何功能和修复任何bug；
build：改变构建流程，新增依赖库、工具等（例如webpack修改）；
style：仅仅修改了空格、缩进等，不改变代码逻辑；
perf：改善性能和体现的修改；
chore：非src和test的修改；
test：测试用例的修改；
ci：自动化流程配置修改；
revert：回滚到上一个版本；

##### scope：【可选】用于说明commit的影响范围

##### subject：commit的简要说明，尽量简短

### Body
对本次commit的详细描述，可分多行

### Footer
不兼容变动：需要描述相关信息
关闭指定Issue：输入Issue信息