---
title: "Athens 简单使用"
date: 2018-09-03T11:44:34+08:00
tags: ["go module"]
categories: ["go"]
---

## 目录
>  
1、本地简单试用 Athens Proxy
2、使用 Docker 部署 Athens Proxy
3、Athens 架构

---

### 1、本地简单试用 Athens Proxy

需要确保安装了 go1.11，并且将 `$GOPATH/bin` 添加到了环境变量，接下来使用 go 工具链安装并运行 Athens proxy

```
# 安装
$ go get -u github.com/gomods/athens/cmd/proxy

# 运行 Athens proxy
$ proxy &
[1] 25243
INFO[0000] Starting application at 127.0.0.1:3000
```

接下来，需要启用 Go Module 功能并配置 Go 使用代理

```
export GO111MODULE=on
export GOPROXY=http://127.0.0.1:3000
```

> 注：启用 Go Module 功能的方法有多种，设置 `GO111MODULE=on` 环境变量只是一种方法，更多的细节自行了解。

现在，就可以构建并运行 walkthrough 项目，go 命令将会通过 Athens proxy 获取依赖

```
$ git clone https://github.com/athens-artifacts/walkthrough.git
$ cd walkthrough
$ go run .
go: finding github.com/athens-artifacts/samplelib v1.0.0
handler: GET /github.com/athens-artifacts/samplelib/@v/v1.0.0.info [200]
handler: GET /github.com/athens-artifacts/samplelib/@v/v1.0.0.mod [200]
go: downloading github.com/athens-artifacts/samplelib v1.0.0
handler: GET /github.com/athens-artifacts/samplelib/@v/v1.0.0.zip [200]
The 🦁 says rawr!
```

> 注： `The 🦁 says rawr!` 是运行结果

> 将 walkthrough 项目的 go.mod 文件摘录在这里

```
module github.com/athens-artifacts/walkthrough

require github.com/athens-artifacts/samplelib v1.0.0
```

### 2、使用 Docker 部署 Athens Proxy

第一节中使用 `proxy &` 命令启动的 Athens Proxy 使用内存作为存储空间(use in-memory storage)，这仅仅适用于简单的试用 Athens Proxy 的代理功能，因为它很快就会 OOM 并且 modules 不会被持久化，重启后就没有了 (This is only suitable for trying out the proxy for a short period of time, as you will quickly run out of memory and Athens won’t persist modules between restarts)

接下来，我们使用 Docker 来运行 Athens Proxy

Athens 支持许多的存储驱动，这里以本地硬盘存储为例，更多的存储驱动需要等到官方出文档(For other providers, please see the Storage Provider documentation [Coming Soon].)

首先创建持久化 modules 的文件夹，如 `/root/athens-storage`，然后运行 Docker 命令

```
docker run -d \
   -v /root/athens-storage:/var/lib/athens \
   -e ATHENS_DISK_STORAGE_ROOT=/var/lib/athens \
   -e ATHENS_STORAGE_TYPE=disk \
   --name athens-proxy \
   --restart always \
   -p 3000:3000 \
   gomods/proxy:latest
```

设置 `ATHENS_STORAGE_TYPE=disk` 和 `ATHENS_DISK_STORAGE_ROOT=/var/lib/athens` 环境变量让 Athens 使用本地硬盘存储。

现在，挂载着 `/root/athens-storage` 数据卷的 athens-proxy 容器启动好了，它监听本地的 3000 端口。Athens 获取到的 modules 都会存储在 `/root/athens-storage` 目录下。

接下来启用 Go Module 功能并配置 Go 使用代理，然后构建并运行 walkthrough 项目

```
$ export GO111MODULE=on
$ export GOPROXY=http://127.0.0.1:3000
$ git clone https://github.com/athens-artifacts/walkthrough.git
$ cd walkthrough
$ go run .
go: finding github.com/athens-artifacts/samplelib v1.0.0
go: downloading github.com/athens-artifacts/samplelib v1.0.0
The 🦁 says rawr!
```

我们可以查看 athens-proxy 容器的日志来验证 Athens 处理了这些请求

```
$ docker logs -f athens-proxy
time="2018-09-03T07:31:19Z" level=warning msg="Unless you set SESSION_SECRET env variable, your session storage is not protected!"
time="2018-09-03T07:31:19Z" level=info msg="Starting application at 0.0.0.0:3000"
handler: GET /github.com/athens-artifacts/samplelib/@v/v1.0.0.info [200]
handler: GET /github.com/athens-artifacts/samplelib/@v/v1.0.0.mod [200]
handler: GET /github.com/athens-artifacts/samplelib/@v/v1.0.0.zip [200]
```

再看一下 `/root/athens-storage` 目录

```
[root@master ~]# tree athens-storage/
athens-storage/
└── github.com
    └── athens-artifacts
        └── samplelib
            └── v1.0.0
                ├── go.mod
                ├── source.zip
                └── v1.0.0.info

4 directories, 3 files
```

它包含了 samplelib module 的文件

go module 机制会将下载的 modules 缓存在 $GOPATH/pkg/mod 目录下，在本机查看一下该目录

![](https://lollipop.xiaosongfu.com/blog/201809/athens.png)

---

* 参考文章：[为 Go module 搭建私服](http://blog.cyeam.com/golang/2018/09/27/athens)