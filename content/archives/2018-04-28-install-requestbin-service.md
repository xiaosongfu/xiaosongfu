---
title: 自己搭建 requestbin 服务
date: 2018-04-28 16:45:08
tags: ["request"]
categories: ["request"]
---

## 目录
>  
1、requestbin 简介  
2、开始安装  
3、其他


### 1、requestbin 简介

requestbin 官网：[requestbin](https://github.com/Runscope/requestbin)

> 1 安装 docker-compose  
```
sudo curl -L https://github.com/docker/compose/releases/download/1.21.0/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```
参考：[install-compose](https://docs.docker.com/compose/install/#install-compose)

> 2 build 镜像  
按照 Github 仓库的 READMe 文档安装即可，摘录如下：

---  

### Deploy your own instance using Docker
On the server/machine you want to host this, you'll first need a machine with docker and docker-compose installed, then grab the RequestBin source using git:
```
$ git clone git://github.com/Runscope/requestbin.git
```
Go into the project directory and then build and start the containers
```
$ sudo docker-compose build
$ sudo docker-compose up -d
```
Your own private RequestBin will be running on this server.

---  

先拉取仓库代码，然后执行 `docker-compose build` ，此时执行 `docker images` ：
```
[root@host requestbin]# docker images
REPOSITORY                                    TAG                 IMAGE ID            CREATED             SIZE
requestbin_app                                latest              687d18309359        13 seconds ago      239MB
python                                        2.7-alpine          b7ebfc836cfe        23 hours ago        73.2MB
```
最后执行 `docker-compose up -d` 启动 docker，此时执行 `docker images` ：
```
[root@host requestbin]# docker images
REPOSITORY                                    TAG                 IMAGE ID            CREATED              SIZE
requestbin_app                                latest              687d18309359        About a minute ago   239MB
python                                        2.7-alpine          b7ebfc836cfe        23 hours ago         73.2MB
redis                                         latest              c5355f8853e4        4 weeks ago          107MB
```
执行 `docker ps` ：
```
[root@host requestbin]# docker ps
CONTAINER ID        IMAGE                                         COMMAND                  CREATED             STATUS              PORTS                    NAMES
bd5b2a6024d7        requestbin_app                                "/bin/sh -c 'gunicor…"   9 minutes ago       Up 9 minutes        0.0.0.0:8000->8000/tcp   requestbin_app_1
23d0565e4a68        redis                                         "docker-entrypoint.s…"   9 minutes ago       Up 9 minutes        6379/tcp                 requestbin_redis_1
```

