---
title: "安装单机器 drone"
date: 2019-01-31T22:55:39+08:00
tags: ["drone"]
categories: ["drone"]
---

原文：[Single Machine](https://docs.drone.io/installation/gitlab/single-machine/)

---

本文档的目标是为您提供足够的技术细节，以便在单机模式下配置和运行 Drone 服务器。Drone 服务器将使用嵌入式 sqlite 数据库，并将在与服务器相同的机器上执行 pipeline。

### 1. 先决条件：创建 OAuth application

创建 GitLab OAuth application。Consumer Key 和 Consumer Secret 用于授权访问 Bitbucket 资源。授权回调 URL 必须与以下格式和路径匹配，并且必须使用您的确切服务器 scheme 和 host。


### 2. 下载服务器
Drone 服务器以轻量级 Docker image 分发。该 image 是自包含的，没有任何外部依赖项。

```
$ docker pull drone/drone:1.0.0-rc.5
```

### 3. 启动服务器
可以使用以下命令启动服务器容器。通过环境变量来配置容器。有关配置参数的完整列表，请参阅 [配置参考](https://docs.drone.io/reference/)。

```
$ docker run \
  --volume=/var/run/docker.sock:/var/run/docker.sock \
  --volume=/var/lib/drone:/data \
  --env=DRONE_GIT_ALWAYS_AUTH=false \
  --env=DRONE_GITLAB_SERVER=https://gitlab.com \
  --env=DRONE_GITLAB_CLIENT_ID={% your-gitlab-client-id %} \
  --env=DRONE_GITLAB_CLIENT_SECRET={% your-gitlab-client-secret %} \
  --env=DRONE_RUNNER_CAPACITY=2 \
  --env=DRONE_SERVER_HOST={% your-drone-server-host %} \
  --env=DRONE_SERVER_PROTO={% your-drone-server-protocol %} \
  --env=DRONE_TLS_AUTOCERT=false \
  --publish=80:80 \
  --publish=443:443 \
  --restart=always \
  --detach=true \
  --name=drone \
  drone/drone:1.0.0-rc.5
```

我的运行命令：

```
$ docker run -d \
  --name=drone \
  --volume=/var/run/docker.sock:/var/run/docker.sock \
  --volume=/var/lib/drone:/data \
  --env=DRONE_GIT_ALWAYS_AUTH=false \
  --env=DRONE_GITLAB_SERVER=http://git.ibdbots.com \
  --env=DRONE_GITLAB_CLIENT_ID=1eca36316a92c00967b69d57c55196885ed1f1de2f53189a4272d516d2d5675d \
  --env=DRONE_GITLAB_CLIENT_SECRET=b88318a113d215bebf90dcda7b541bce93718a68cb8e7e50505892f353720fd5 \
  --env=DRONE_RUNNER_CAPACITY=2 \
  --env=DRONE_SERVER_HOST=drone.xiaosongfu.com \
  --env=DRONE_SERVER_PROTO=https \
  --env=DRONE_TLS_AUTOCERT=true \
  --publish=80:80 \
  --publish=443:443 \
  --restart=always \
  --detach=true \
  drone/drone:1.0.0-rc.5
```

### 4. 配置参考
本节提供了本文档前面使用到的配置变量的补充说明。这只是配置参数的一部分(子集)。有关配置参数的完整列表，请参阅 [配置参考](https://docs.drone.io/reference/)。

### 5. Docker 参考

---

参考：

1. [基于drone的CI/CD实践，对接kubernetes，见证灵活与自由](https://blog.csdn.net/github_35614077/article/details/83028832)