---
title: 安装 golang.org/x/ 下的 package
date: 2018-02-24 10:28:46
tags: ["golang"]
categories: ["golang"]
---

## 目录
>  
1、从 github.com 下载  
2、设置命令行代理  


### 1、从 github.com 下载
github.com/golang 组织将 golang.org/x/ 下的包都 mirror 了一份，从这直接下载并保存到正确的目录即可完成安装。可按需执行 `go install` 来达到和 `go get` 一样的效果。  

使用如下命令建立好目录：

```
mkdir -p $GOPATH/src/golang.org/x/
```

然后切换到该目录后使用如下命令下载包文件(以 sys 包为例)：

```
git clone https://github.com/golang/sys.git
```

需要更新包文件的时候，切换到对应目录，然后执行以下命令即可:

```
git pull origin master
```

### 2、设置命令行代理
使用如下命令设置命令行代理：

```
export http_proxy=http://127.0.0.1:1087;export https_proxy=http://127.0.0.1:1087;
```

其中 http://127.0.0.1:1087 是你的本地代理服务，需要保证该服务器可用!需要保证该服务器可用!需要保证该服务器可用!  
然后就可以正常使用 go get 来下载了：

```
go get -u -v golang.org/x/vgo
```

> 其他  

* 查看命令行代理设置

```
echo $http_proxy
echo $https_proxy
```

* 取消命令行代理设置：

```
unset http_proxy;unset https_proxy;
```
