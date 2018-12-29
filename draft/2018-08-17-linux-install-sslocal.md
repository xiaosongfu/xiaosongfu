---
title: ""
date: 2018-08-17T10:27:53+08:00
tags: []
categories: []
---

https://www.jianshu.com/p/824912d9afda


```
yum -y install epel-release
yum -y install python-pip
pip install shadowsocks
pip install --upgrade pip

vi /etc/shadowsocks.json
nohup sslocal -c /etc/shadowsocks.json  </dev/null 2>&1 &
ps aux |grep sslocal |grep -v "grep"


curl --socks5 127.0.0.1:1087 http://httpbin.org/ip


yum -y install privoxy
vi /etc/privoxy/config
systemctl start privoxy


export http_proxy=http://127.0.0.1:8118
export https_proxy=http://127.0.0.1:8118


curl ip.cn
curl ip.sb
curl www.google.com
```

unset http_proxy;unset https_proxy;

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


```
# listen-address 127.0.0.1:8118 # 没有加这个才可以

forward-socks5 / 127.0.0.1:1087 . # 注意最后有个点 .
```