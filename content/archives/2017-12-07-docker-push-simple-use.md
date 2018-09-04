---
title: docker push 简记
date: 2017-12-07 16:18:18
tags: ["docker","push"]
categories: ["docker"]
---

## 目录
>  
1、简记   
2、注意事项


### 1、简记  
使用 docker push 命令将 image 推送到 Docker Hub 之前需要现在 Docker Hub 上注册账号并在命令行登录完成，输入 docker login 后回车，并按照提示输入用户名和密码即可登录成功。  

如果直接推送 image ，则会报错，如下：

```  
[root@MyCloudServer pos-master]# docker push pos
The push refers to a repository [docker.io/library/pos]
c4c67b92de11: Preparing 
50f193fb6c79: Preparing 
d7e0a9307630: Preparing 
f73d86c10bc2: Preparing 
c59fa6cbcbd9: Preparing 
8d4d1ab5ff74: Waiting 
denied: requested access to the resource is denied
[root@MyCloudServer pos-master]# 
```  

原因是因为 image 的 REPOSITORY 属性需要和你的 Docker Hub 用户名相关，比如：xiaosongfu/pos，而不能只是 pos 。可以通过添加 tag 的方法来解决。添加 tag 的时候可以显式指定一个 tag ，也可以不指定而使用默认的 latest。先看一下 docker tag 命令的使用方法：  

```  ·
[root@MyCloudServer pos-master]# docker tag --help

Usage:	docker tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]

Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE

Options:
      --help   Print usage
[root@MyCloudServer pos-master]# 
```  

Usage 里的 [:TAG] 就是说 tag 是可选的。接着执行如下命令为 image 打上 tag：  

```  
[root@MyCloudServer pos-master]# docker tag freebb xiaosongfu/pos
```  
或  
```  
[root@MyCloudServer pos-master]# docker tag freebb xiaosongfu/pos:1.0
```  
其中的 xiaosongfu 为你的用户名。第一条命令使用默认的 latest tag ，第二条命令显式指定了一个 tag ：1.0 。     

命令执行成功后可以使用 docker images 命令查看对应 tag 的 image：  

```  
[root@MyCloudServer pos-master]# docker images
REPOSITORY                              TAG                 IMAGE ID            CREATED             SIZE
pos                                     latest              905f6c95abb0        20 minutes ago      216MB
xiaosongfu/pos                          1.0                 905f6c95abb0        20 minutes ago      216MB
xiaosongfu/pos                          latest              905f6c95abb0        20 minutes ago      216MB
[root@MyCloudServer pos-master]# 
```  

接着就可以开始推送了，使用如下命令：  

```  
[root@MyCloudServer pos-master]# docker push xiaosongfu/pos
```  
或  
```  
[root@MyCloudServer pos-master]# docker push xiaosongfu/pos:1.0
```  
第一条命令就是将默认的 latest tag 对应的 image 推送到 Docker Hub ，第二条是将 tag 为 1.0 的 image 推送到 Docker Hub。  

推送成功后登录 Docker Hub 查看，结果如下： 

![dockerposimages.png](https://lollipop.xiaosongfu.com/blog/201712/dockerposimages.png)  

### 2、注意事项  
* 不需要先在 Docker Hub 官网上点击 “Create Repository” 创建一个 Repository   
* 执行 docker tag 命令时没有加 [:tag] ,（如 docker tag pos xiaosongfu/pos ），则在拉取该 image 时可以不带 tag （默认使用 latest ）或显式指定 tag 为 latest（如 docker pull xiaosongfu/pos 或 如 ocker pull xiaosongfu/pos:latest ）
* 执行 docker tag 命令时添加了 [:tag] ，（如 docker tag pos xiaosongfu/pos:1.0 ），则在拉取该 image 时必须带上对应的 tag （如 docker pull xiaosongfu/pos:1.0 ），否则就是拉取 tag 为 latest 的那个 image