---
title: 快速登录 linux 服务器
date: 2018-04-28 13:59:45
tags: ["linux","login"]
categories: ["linux"]
---

## 目录
>  
1、使用 alias 命令  
2、使用 ~/.ssh/config  
3、可选：配置免密码登录


### 1、使用 alias 命令  

使用 alias 命令为登录命令起一个简短的名字，如：
```
alias 别名=`ssh 用户名@ip地址 -p 端口`
```
执行成功后直接在终端输入 `别名` 回车并输入密码即可登录。  

> 注意：  
* 要删除一个别名，可以使用 unalias 命令，如 `unalias 别名`。  
* alias 命令的作用只局限于该次登入的操作。若要每次登入都能够使用这些命令别名，则可将相应的 alias 命令存放到 bash 的初始化文件 `/etc/bashrc` 中。

### 2、使用 ~/.ssh/config   

编辑 ~/.ssh/config 文件，加入如下内容：
```
Host 别名
Hostname ip地址
Port 端口
User 用户名
```
而配置好之后使用 `ssh 别名` 回车并输入密码即可登录。

### 3、可选：配置免密码登录
使用前面的两种方式都需要输入密码，如果需要免密码登录只需要将本机生成的 rsa 公钥拷贝到服务器上即可。具体步骤如下：  
* 1、生成秘钥
```
ssh-keygen -t rsa
```

生成的密钥在 ~/.ssh/ 下面:
```
fuxiaosongdeMac-mini:~ fuxiaosong$ ls -l ~/.ssh/
total 32
-rw-r--r--  1 fuxiaosong  staff    54  4 28 14:11 config
-rw-------  1 fuxiaosong  staff  3243  4  5  2017 id_rsa
-rw-r--r--  1 fuxiaosong  staff   732  4  5  2017 id_rsa.pub
-rw-r--r--  1 fuxiaosong  staff  3975  4 27 15:16 known_hosts
```

* 2、将 id_rsa.pub 的内容写入服务器的登录用户名的家目录下的 .ssh/authorized_keys 里：  
```
# 拷贝公钥文件到服务器
scp -P 端口 ~/.ssh/id_rsa.pub username@hostname:~/.ssh/id_rsa.pub

# 登录服务器
# ...

# 登录服务器后执行以下命令将公钥写 authorized_keys 文件
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
```

至此，就可以免密码登录了 ^_^
