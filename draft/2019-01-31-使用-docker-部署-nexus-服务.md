---
title: "安装单机器 drone"
date: 2019-01-31T22:58:39+08:00
tags: ["drone"]
categories: ["drone"]
---

## 使用 docker 启动 nexus 服务

启动命令：

```
docker run -d -p 8081:8081 --name nexus sonatype/nexus3
```

默认的账号密码为：`admin` / `admin123`

大概需要2-3分钟的时间启动一个新的容器，可以使用如下命令查看容器日志：

```
docker logs -f nexus
```

## Nexus的仓库与仓库组

Nexus包含了各种类型的仓库概念，包括：
* 代理仓库 (proxy)
* 宿主仓库 (hosted)
* 仓库组等 (group)

![](https://img-blog.csdn.net/20170521190445839?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvTF9TYWls/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

---

参考：

* [Sonatype Nexus3 Docker: sonatype/nexus3](https://hub.docker.com/r/sonatype/nexus3)
* [Nexus的仓库与仓库组](https://blog.csdn.net/l_sail/article/details/72614623)