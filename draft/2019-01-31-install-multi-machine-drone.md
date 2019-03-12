---
title: "安装多机器 drone"
date: 2019-01-31T22:51:39+08:00
tags: ["drone"]
categories: ["drone"]
---

原文：[Multi-Machine](https://docs.drone.io/installation/gitlab/multi-machine/) & [Linux amd64](https://docs.drone.io/administration/agents/linux-amd64/)

---

### 1. 先决条件：创建 OAuth application

略

### 2. 创建共享密钥

创建共享密钥以验证 agents 与 Central Drone Server 之间的通信。使用 `DRONE_RPC_SECRET` 环境变量将此共享密钥传递给 Central Drone Server 和 agents。

您可以使用 openssl 生成共享密钥：

```
$ openssl rand -hex 16
```

### 3. 下载服务器

略

### 4. 启动 Central Drone Server

可以使用以下命令启动服务器容器。通过环境变量来配置容器。有关配置参数的完整列表，请参阅 配置参考。

```
$ docker run \
  --volume=/var/lib/drone:/data \
  --env=DRONE_GIT_ALWAYS_AUTH=false \
  --env=DRONE_GITLAB_SERVER=https://gitlab.com \
  --env=DRONE_GITLAB_CLIENT_ID={% your-gitlab-client-id %} \
  --env=DRONE_GITLAB_CLIENT_SECRET={% your-gitlab-client-secret %} \
  --env=DRONE_RPC_SECRET={% your-shared-secret %} \
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
  --volume=/var/lib/drone:/data \
  --env=DRONE_GIT_ALWAYS_AUTH=false \
  --env=DRONE_GITLAB_SERVER=http://git.ibdbots.com \
  --env=DRONE_GITLAB_CLIENT_ID=1eca36316a92c00967b69d57c55196885ed1f1de2f53189a4272d516d2d5675d \
  --env=DRONE_GITLAB_CLIENT_SECRET=b88318a113d215bebf90dcda7b541bce93718a68cb8e7e50505892f353720fd5 \
  --env=DRONE_RUNNER_CAPACITY=2 \
  --env=DRONE_SERVER_HOST=drone.xiaosongfu.com \
  --env=DRONE_SERVER_PROTO=https \
  --env=DRONE_TLS_AUTOCERT=true \
  --env=DRONE_RPC_SECRET=bea26a2221fd8090ea38720fc445eca6 \
  --publish=80:80 \
  --publish=443:443 \
  --restart=always \
  --detach=true \
  drone/drone:1.0.0-rc.5
```

只是比单机版的多了 `--env=DRONE_RPC_SECRET=bea26a2221fd8090ea38720fc445eca6 \` 这个环境变量。

不需要 `
  --volume=/var/run/docker.sock:/var/run/docker.sock \` 是因为 Central Drone Server 不执行 pipeline，所以不需要这个。

---

### 5. 启动 agent

Drone agent 作为轻量级 Docker image 分发。该 image 是自包含的，没有任何外部依赖项。可以使用以下命令下载并启动代理。

```
$ docker run \
  --volume=/var/run/docker.sock:/var/run/docker.sock \
  --env=DRONE_RPC_SERVER=${DRONE_RPC_SERVER} \
  --env=DRONE_RPC_SECRET=${DRONE_RPC_SECRET} \
  --env=DRONE_RUNNER_CAPACITY=2 \
  --env=DRONE_RUNNER_NAME=${HOSTNAME} \
  --restart=always \
  --detach=true \
  drone/agent:1.0.0-rc.5
```

---

我的运行命令：

```
$ docker run -d \
  --name=drone-agent \
  --volume=/var/run/docker.sock:/var/run/docker.sock \
  --env=DRONE_RPC_SERVER=drone.xiaosongfu.com \
  --env=DRONE_RPC_SECRET=bea26a2221fd8090ea38720fc445eca6 \
  --env=DRONE_RUNNER_CAPACITY=2 \
  --env=DRONE_RUNNER_NAME=drone-agent-us \
  --restart=always \
  --detach=true \
  --name=agent \
  drone/agent:1.0.0-rc.5
```

使用了 `--volume=/var/run/docker.sock:/var/run/docker.sock`，而不使用 `--volume=/var/run/docker.sock:/var/run/docker.sock \`，是因为 agent 只负责执行 pipeline，而不负责管理工作。

* `DRONE_RPC_SERVER` 指向 Central Drone Server，该字符串必须是有效的 URL，并且应该省略尾部斜杠。
* `DRONE_RPC_SECRET` 使用和 Central Drone Server 一样的共享密钥。
* `DRONE_RUNNER_NAME` 为主机名，会被每一次构建存储下来。是可选的，默认为容器的 `HOSTNAME`。

---