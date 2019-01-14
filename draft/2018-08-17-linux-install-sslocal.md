---
title: "Centos 安装 shadowsocks 并将 http/https 代理转发到本地 socks5 代理"
date: 2018-08-17T10:27:53+08:00
tags: ["shadowsocks","socks5","proxy"]
categories: ["shadowsocks"]
---

## 目录
>  
1、安装 shadowsocks 实现 socks5 代理
2、安装 privoxy 实现将本地的 http/https 代理转发到本地 socks5 代理

参考文章：https://www.jianshu.com/p/824912d9afda

## 1、安装 shadowsocks 实现 socks5 代理

```
yum -y install epel-release
yum -y install python-pip
```

可能需要升级 pip：

```
pip install --upgrade pip
```

安装 shadowsocks：

```
pip install shadowsocks
```

配置 shadowsocks：

```
vi /etc/shadowsocks.json
```

内容如下：

```
{
    "server":"ss2.1205.fun",
    "server_port":12051,
    "local_address": "127.0.0.1",
    "local_port":1087,
    "password":"Y2U2N2MzYz",
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open": false,
    "workers": 1
}
```

启动 sslocal：

```
nohup sslocal -c /etc/shadowsocks.json  </dev/null 2>&1 &
```

查看 sslocal 是否启动成功：

```
ps aux |grep sslocal |grep -v "grep"
```

查看 socks5 代理是否成功：

```
[root@DockerApp ~]# curl http://httpbin.org/ip
{
  "origin": "222.85.230.14"
}
[root@DockerApp ~]# curl --socks5 127.0.0.1:1087 http://httpbin.org/ip
{
  "origin": "45.78.12.33"
}
[root@DockerApp ~]#
```

## 2、安装 privoxy 实现将本地的 http/https 代理转发到本地 socks5 代理

安装 privoxy：

```
yum -y install privoxy
```

配置 privoxy：

```
vi /etc/privoxy/config
```

在文件最后添加如下内容：

```
# listen-address 127.0.0.1:8118 # 没有加这个才可以

forward-socks5 / 127.0.0.1:1087 . # 注意最后有个点 .
```

启动 privoxy：

```
systemctl start privoxy
```

配置本地 http/https 代理：

```
export http_proxy=http://127.0.0.1:8118
export https_proxy=http://127.0.0.1:8118
```

测试：

```
[root@DockerApp ~]# curl http://httpbin.org/ip
{
  "origin": "45.78.12.33"
}
[root@DockerApp ~]#
```

类似命令：

```
curl http://httpbin.org/ip
curl ip.cn
curl ip.sb
curl www.google.com
```

取消代理：

```
unset http_proxy;unset https_proxy;
```