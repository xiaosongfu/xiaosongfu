---
title: "Rclone 简单使用"
date: 2018-07-31T16:28:05+08:00
tags: ["rclone"]
categories: ["rclone"]
---

## 目录
>  
1、安装  
2、简单使用  
3、在 Linux 服务器之间同步文件  


官网：[https://rclone.org](https://rclone.org/)  

### 1、安装

安装方式有两种：

> 方式一：

根据操作系统在官网下载对应 arch 包并解压即可得到可执行文件。

![](https://lollipop.xiaosongfu.com/blog/201807/rclone-arch-download.png)  

> 方式二：

直接执行 shell 脚本，自动下载安装

```
curl https://rclone.org/install.sh | sudo bash
```

![](https://lollipop.xiaosongfu.com/blog/201807/rclone-shell-download.png)  


### 2、简单使用

* 配置云盘/服务器

```
rclone config
```

* 列出某目录下的所有目录

```
rclone lsd remote:/path/to/directory
```

* 列出某目录下的所有内容

```
rclone ls remote:/path/to/directory
```

* 创建目录

```
rclone mkdir remote:/path/to/directory
```

* 同步文件夹/文件

```
rclone sync /home/local/directory remote:/path/to/directory
```

### 3、在 Linux 服务器之间同步文件

同步文件夹使用 `rclone sync`，比如：

```
rclone sync /Volumes/semifu/dl jdcloud:/root/app/dl
```

该命令实现将本机的 `/Volumes/semifu/dl` 目录和 `jdcloud` 服务器的 `/root/app/dl` 目录同步。


### 参考

实现同样功能的软件：https://mutagen.io/