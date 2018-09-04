---
title: "Unison 简单使用"
date: 2018-08-07T15:39:31+08:00
tags: ["unison"]
categories: ["unison"]
---


### 下载安装 OCaml 语言的编译器

http://ocaml.org/docs/install.html

![你看不到我^_^](https://lollipop.xiaosongfu.com/blog/201808/ocaml.png)   

```
yum install ocaml
```

### 下载 unison 源码并编译安装

下载 unison 源码

https://github.com/bcpierce00/unison/releases

```
wget https://github.com/bcpierce00/unison/archive/v2.51.2.zip
```

编译安装

http://www.seas.upenn.edu/~bcpierce/unison/download/releases/stable/unison-manual.html#building

```
make UISTYLE=text
make install
```

如果 `make install` 命令报错，如下:

```
[root@host unison-2.51.2]# make install
(cd src; make install)
make[1]: 进入目录“/root/unison-2.51.2/src”
UISTYLE = text
Building for Unix
NATIVE = true
THREADS = false
STATIC = false
OSTYPE =
OSARCH = Linux
mv /root/bin//unison /tmp/unison-1473
mv: 无法获取"/root/bin//unison" 的文件状态(stat): 没有那个文件或目录
make[1]: [doinstall] 错误 1 (忽略)
cp unison /root/bin/
cp: 无法创建普通文件"/root/bin/": 不是目录
make[1]: *** [doinstall] 错误 1
make[1]: 离开目录“/root/unison-2.51.2/src”
make: *** [install] 错误 2
[root@host unison-2.51.2]#
```

则手动创建 `/root/bin` 目录，使用命令：`mkdir /root/bin`


最后将 `/root/bin/unison` 移动到 `/usr/local/bin/` 目录下，使用如下命令：

```
cp bin/unison /usr/local/bin/
```

如果不执行这一步，在同步到另一台服务器的时候会提示另一台服务器上找不到 `unison` 命令，如：

```
[root@JD ~]# bin/unison
Unison 2.51.2 (ocaml 4.05.0): Contacting server...
bash: unison: 未找到命令
Fatal error: Lost connection with the server
[root@JD ~]#
```

### 配置双机 ssh 信任 

```
[root@host ~]# ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/root/.ssh/id_rsa):
Created directory '/root/.ssh'.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /root/.ssh/id_rsa.
Your public key has been saved in /root/.ssh/id_rsa.pub.
The key fingerprint is:
59:ba:ad:f4:5e:30:4a:2c:84:e4:f6:85:1c:fb:1f:26 root@host.localdomain
The key's randomart image is:
+--[ RSA 2048]----+
|    . .          |
|   o o +         |
|    + = . .      |
|   . o + +       |
|      o E =      |
|       o B +     |
|        + o .    |
|       . o .     |
|        ..o      |
+-----------------+
[root@host ~]# cd .ssh
[root@host .ssh]# ls
id_rsa  id_rsa.pub
[root@host .ssh]# scp ~/.ssh/id_rsa.pub root@117.48.200.231:/root
```

在 117.48.200.231 上将

```
cat /root/id_rsa.pub >> ~/.ssh/authorized_keys
```

同样的步骤在另一台电脑上配置好。

### 通过配置文件来使用 unison

使用 root 安装 unison 后，配置文件默认生成为 `/root/.unison/default.prf`,也可以手动写一个配置文件，运行 unison 时指定配置文件即可。 

默认生成的 `/root/.unison/default.prf` 配置文件是空的。

修改配置文件如下：

```
# Unison preferences file

root = /root/app/dl

root = ssh://root@45.78.12.33:29086//root/app/dl

rsync = false

batch = true
fastcheck = true

log = true
logfile = /root/.unison/unison.log
```


配置文件配置好之后使用 `unison` 命令即可开始同步。