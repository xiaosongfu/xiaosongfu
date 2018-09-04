---
title: "安装 Golang"
date: 2018-06-26T17:54:16+08:00
tags: ["golang"]
categories: ["golang"]
---

安装 golang 十分的简单，只需要下载 archive 包，解压，设置 GOROOT 和 GOPATH 环境变量，并将它们的 bin 目录添加到系统 PATH 环境变量即可。  

> 以 Linux 系统为例，安装 1.10.2 版本(已使用 root 用户登录服务器)

##### 1.创建保存 golang 的目录

```
mkdir -p /root/dev/go/gopath
cd /root/dev/go
```

创建 go 和 go/gopath 目录，安装完成后的目录结构会是这样的：

```
[root@JD ~]# tree -L 4
.
└── dev
    └── go
        ├── go
        │   ├── api
        │   ├── AUTHORS
        │   ├── bin
        │   ├── blog
        │   ├── CONTRIBUTING.md
        │   ├── CONTRIBUTORS
        │   ├── doc
        │   ├── favicon.ico
        │   ├── lib
        │   ├── LICENSE
        │   ├── misc
        │   ├── PATENTS
        │   ├── pkg
        │   ├── README.md
        │   ├── robots.txt
        │   ├── src
        │   ├── test
        │   └── VERSION
        ├── go1.10.2.linux-amd64.tar.gz
        └── gopath

13 directories, 10 files
```

##### 2.下载 golang archive 包。国内官网：[https://golang.google.cn/dl/](https://golang.google.cn/dl/)

```
wget https://dl.google.com/go/go1.10.2.linux-amd64.tar.gz
```

下载地址是有规律的： `go$VERSION.$OS-$ARCH.tar.gz`，所以安装新版本的时候可以直接拼接下载地址直接下载。

> 这个下载地址需要 FQ ，可以到笔者搭建的 Mirrors 站点下载：[https://mirrors.xiaosongfu.com](https://mirrors.xiaosongfu.com/#/golang/golang)

##### 3.下载完成之后解压

```
tar -C ~/dev/go -xzf go1.10.2.linux-amd64.tar.gz
```

##### 4.设置 GOROOT 和 GOPATH 环境变量，并将它们的 bin 目前加到系统 PATH 环境变量

> You can do this by adding this line to your /etc/profile (for a system-wide installation) or $HOME/.profile ---- 摘自官网

配置环境变量可以使用 `/etc/profile` 文件或 `$HOME/.profile` 文件，这里以 `/etc/profile` 为例：

```
vi /etc/profile
```

在文件中添加一下内容：

```
export GOROOT="/root/develop/go/go"
export GOPATH="/root/develop/go/gopath"

export PATH=$PATH:$GOROOT/bin:$GOPATH/bin
```

首先设置了 GOROOT 和 GOPATH 环境变量，然后将它们的 bin 目录都添加到了系统 PATH 环境变量。

接着执行 `source /etc/profile` 命令让配置生效：

```
source /etc/profile
```

##### 5.测试安装是否成功

```
go version
```