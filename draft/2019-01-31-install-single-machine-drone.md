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

![](https://lollipop.xiaosongfu.com/blog/201901/gitlab_token_create.png)

![](https://lollipop.xiaosongfu.com/blog/201901/gitlab_token_created.png)

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

---

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

![](https://lollipop.xiaosongfu.com/blog/201901/drone-gitlab-config.png)

* `DRONE_GIT_ALWAYS_AUTH`：配置 Drone 在克隆公共存储库时进行身份验证。此配置参数的类型为 boolean，只有在源代码管理系统（例如GitHub Enterprise）启用了私有模式时才需要这样做。如：`DRONE_GIT_ALWAYS_AUTH=false`
* `DRONE_SERVER_HOST`：必需的字符串字面量，指定 Drone 服务器主机名，如：`DRONE_SERVER_HOST=drone.company.com`
* `DRONE_SERVER_PORT`：字符串字面量，指定侦听传入 http 连接时使用的端口。您永远不需要覆盖此值。只应在容器外执行 Drone 二进制文件时设置此值。如：`DRONE_SERVER_PORT=:80`
* `DRONE_SERVER_PROTO`：必需的字符串字面量，指定 Drone 服务器协议，如：`DRONE_SERVER_PROTO=https`
* `DRONE_TLS_AUTOCERT`：使用 Lets Encrypt 自动生成 SSL 证书，并将服务器配置为接受 HTTPS 请求。此配置参数的类型为 boolean，是可选的，默认情况下处于禁用状态。如：`DRONE_TLS_AUTOCERT=true`
* `DRONE_TLS_CERT`：服务器用于接受HTTPS请求的SSL证书的路径。此配置参数的类型为 string，并且是可选的。如：`DRONE_TLS_CERT=/path/to/cert.pem`
* `DRONE_TLS_KEY`：服务器用于接受HTTPS请求的SSL证书密钥的路径。此配置参数的类型为string，并且是可选的。如：`DRONE_TLS_KEY=/path/to/key.pem`

更多的环境变量请参考：[Server Reference](https://docs.drone.io/reference/server/)

---

### 4. 配置参考

本节提供了本文档前面使用到的配置变量的补充说明。这只是配置参数的一部分(子集)。有关配置参数的完整列表，请参阅 [配置参考](https://docs.drone.io/reference/)。

### 5. Docker 参考

---

参考：

1. [基于drone的CI/CD实践，对接kubernetes，见证灵活与自由](https://blog.csdn.net/github_35614077/article/details/83028832)