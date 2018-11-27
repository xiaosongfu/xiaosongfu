---
Title: "Caddy 简单实用"
Date: 2018-08-06T15:55:18+08:00
tags: ["caddy"]
categories: ["caddy"]
---

## 目录
>  
1、安装  
2、创建配置文件  


### 安装

```
[root@JD ~]# curl https://getcaddy.com | bash -s personal
```

创建相关目录

```
[root@JD ~]# mkdir /etc/caddy
[root@JD ~]# mkdir /etc/ssl/caddy
[root@JD ~]# mkdir /var/log/caddy
```

### 创建配置文件

```
[root@JD ~]# vi /etc/caddy/Caddyfile
```


### 启动服务

```
[root@JD ~]# caddy -conf /etc/caddy/Caddyfile
```