---
title: "自动拼接 url 下载 github 特定项目的 release"
date: 2019-01-31T23:31:39+08:00
tags: ["drone"]
categories: ["drone"]
---

首先获取当前系统的类型：

```
$ export OS=$(uname -s| tr '[:upper:]' '[:lower:]')
# 或者
$ export OS="$(uname)"
```

获取项目的最新 release 版本号，以 kubeless 项目为例：

```
$ export RELEASE=$(curl -s https://api.github.com/repos/kubeless/kubeless/releases/latest | grep tag_name | cut -d '"' -f 4)
```

现在开始拼接 url。首先 `https://github.com/kubeless/kubeless/releases/download/` 这部分是固定的，然后看一下项目 release 页面下载文件的命名方式，如：`kubeless_linux-amd64.zip` ，则可以得到最终的 url 如下：

```
https://github.com/kubeless/kubeless/releases/download/$RELEASE/kubeless_$OS-amd64.zip
```


