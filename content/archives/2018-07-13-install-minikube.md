---
title: "主流操作系统安装 minikube"
date: 2018-07-13T11:42:21+08:00
tags: ["kubernets","minikube"]
categories: ["kubernets"]
---

## 目录
>  
1、macOS 系统安装 Minikube  
2、Linux 系统安装 Minikube  

Minikube Github：[https://github.com/kubernetes/minikube](https://github.com/kubernetes/minikube)  
Minikube Github Release：[https://github.com/kubernetes/minikube/releases](https://github.com/kubernetes/minikube/releases)  

* 本文整理自 [https://github.com/kubernetes/minikube/releases](https://github.com/kubernetes/minikube/releases)

---

### macOS 系统安装 minikube

有两种安装方式:

> 方式一: 使用 brew 安装：

```
brew cask install minikube
```

> 方式二: 直接下载

```
curl -Lo minikube https://storage.googleapis.com/minikube/releases/v0.28.0/minikube-darwin-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/
```

这个命令包含三个部分：

1. `curl -Lo minikube https://storage.googleapis.com/minikube/releases/v0.28.0/minikube-darwin-amd64` => 下载 minikube-darwin-amd64 并重命名为 minikube  
2. `chmod +x minikube` => 给 minikube 赋予执行权限
3. `mv minikube /usr/local/bin/` => 将 minikube 移动到 `/usr/local/bin/` 目录下，这一步是可选的，你也可以直接手动将 minikube 的路径添加到系统的环境变量

```
fuxiaosongdeMac-mini:~ fuxiaosong$ minikube
Minikube is a CLI tool that provisions and manages single-node Kubernetes clusters optimized for development workflows.

Usage:
  minikube [command]

Available Commands:
  addons           Modify minikube's kubernetes addons
  cache            Add or delete an image from the local cache.
  completion       Outputs minikube shell completion for the given shell (bash or zsh)
  config           Modify minikube config
  dashboard        Opens/displays the kubernetes dashboard URL for your local cluster
  delete           Deletes a local kubernetes cluster
  docker-env       Sets up docker env variables; similar to '$(docker-machine env)'
  get-k8s-versions Gets the list of Kubernetes versions available for minikube when using the localkube bootstrapper
  help             Help about any command
  ip               Retrieves the IP address of the running cluster
  logs             Gets the logs of the running localkube instance, used for debugging minikube, not user code
  mount            Mounts the specified directory into minikube
  profile          Profile sets the current minikube profile
  service          Gets the kubernetes URL(s) for the specified service in your local cluster
  ssh              Log into or run a command on a machine with SSH; similar to 'docker-machine ssh'
  ssh-key          Retrieve the ssh identity key path of the specified cluster
  start            Starts a local kubernetes cluster
  status           Gets the status of a local kubernetes cluster
  stop             Stops a running local kubernetes cluster
  update-check     Print current and latest version number
  update-context   Verify the IP address of the running cluster in kubeconfig.
  version          Print the version of minikube

Flags:
      --alsologtostderr                  log to standard error as well as files
  -b, --bootstrapper string              The name of the cluster bootstrapper that will set up the kubernetes cluster. (default "kubeadm")
  -h, --help                             help for minikube
      --log_backtrace_at traceLocation   when logging hits line file:N, emit a stack trace (default :0)
      --log_dir string                   If non-empty, write log files in this directory
      --loglevel int                     Log level (0 = DEBUG, 5 = FATAL) (default 1)
      --logtostderr                      log to standard error instead of files
  -p, --profile string                   The name of the minikube VM being used.
	This can be modified to allow for multiple minikube instances to be run independently (default "minikube")
      --stderrthreshold severity         logs at or above this threshold go to stderr (default 2)
  -v, --v Level                          log level for V logs
      --vmodule moduleSpec               comma-separated list of pattern=N settings for file-filtered logging

Use "minikube [command] --help" for more information about a command.
fuxiaosongdeMac-mini:~ fuxiaosong$
```

### Linux 系统安装 Minikube

```
curl -Lo minikube https://storage.googleapis.com/minikube/releases/v0.28.2/minikube-linux-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/
```

这个命令包含三个部分：

1. `curl -Lo minikube https://storage.googleapis.com/minikube/releases/v0.28.2/minikube-linux-amd64` => 下载 minikube-linux-amd64 并重命名为 minikube  
2. `chmod +x minikube` => 给 minikube 赋予执行权限
3. `mv minikube /usr/local/bin/` => 将 minikube 移动到 `/usr/local/bin/` 目录下，这一步是可选的，你也可以直接手动将 minikube 的路径添加到系统的环境变量

---

官网 minikube 文档阅读指南：

1. 安装：Install Minikube --- https://kubernetes.cn/docs/tasks/tools/install-minikube/
2. 使用：Running Kubernetes Locally via Minikube --- https://kubernetes.cn/docs/setup/minikube/
3. Tutorials：Hello Minikube --- https://kubernetes.cn/docs/tutorials/hello-minikube/
