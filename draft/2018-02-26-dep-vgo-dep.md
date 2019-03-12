---
title: dep&vgo-dep
date: 2018-02-26 13:59:00
tags: []
categories: []
---

## 目录
>  
1、   
2、

---

### 1、

go 允许 import 不同代码库的代码，例如 github.com, golang.org 等等；对于需要import 的代码，可以使用 `go get` 命令取下来放到 GOPATH 对应的目录中去。


对于go来说，其实并不care你的代码是内部还是外部的，总之都在GOPATH里，任何import包的路径都是从GOPATH开始的；唯一的区别，就是内部依赖的包是开发者自己写的，外部依赖的包是 go get 下来的。


依赖GOPATH来解决go import有个很严重的问题：如果项目依赖的包做了修改，或者干脆删掉了，会影响到其他现有的项目。为了解决这个问题，go在1.5版本引入了vendor属性(默认关闭，需要设置go环境变量GO15VENDOREXPERIMENT=1)，并在1.6版本之后都默认开启了vendor属性。 这样的话，所有的依赖包都在项目工程的vendor中了，每个项目都有各自的vendor，互不影响；但是vendor里面的包没有版本信息，不方便进行版本管理。
