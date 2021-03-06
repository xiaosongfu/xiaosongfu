---
title: "主流操作系统安装 kubectl"
date: 2018-08-16T11:07:23+08:00
tags: ["kubernets","kubectl"]
categories: ["kubernets"]
---

## 目录
>  
1、使用包管理器安装 (Install kubectl binary via native package management)  
2、使用 curl 安装 (Install kubectl binary via curl) 
3、使用 Google Cloud SDK 安装 (Download as part of the Google Cloud SDK)
4、配置 shell 终端自动补齐命令 (Enabling shell autocompletion) 

* 本文整理自 Kubernetes 官网文档：[Install and Set Up kubectl](https://kubernetes.cn/docs/tasks/tools/install-kubectl/)

---

## 1、使用包管理器安装 (Install kubectl binary via native package management)

##### 1.1 Linux 系统上使用包管理器安装

> Ubuntu,Debian or HypriotOS

```
sudo apt-get update && sudo apt-get install -y apt-transport-https
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
sudo touch /etc/apt/sources.list.d/kubernetes.list 
echo "deb http://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee -a /etc/apt/sources.list.d/kubernetes.list
sudo apt-get update
sudo apt-get install -y kubectl
```

> CentOS,RHEL or Fedora

```
cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
EOF
yum install -y kubectl
```

##### 1.2 Ubuntu 系统上使用 snap 安装

```
sudo snap install kubectl --classic
```

安装完成后运行 `kubectl version` 命令验证安装是否成功

##### 1.3 macOS 系统上使用 homebrew 安装

```
brew install kubernetes-cli
```

安装完成后运行 `kubectl version` 命令验证安装是否成功

## 2、使用 curl 安装 (Install kubectl binary via curl) 

> macOS

首先下载 kubectl 可执行文件：

```
curl -LO https://storage.googleapis.com/kubernetes-release/release/{version}/bin/darwin/amd64/kubectl
```

其中 {version} 部分需要使用指定的版本代替，如 v1.11.0： `curl -LO https://storage.googleapis.com/kubernetes-release/release/v1.11.0/bin/darwin/amd64/kubectl`

然后赋予可执行权限：

```
chmod +x ./kubectl
```

最后将其移动到 `/usr/local/bin` 目录下：

```
sudo mv ./kubectl /usr/local/bin/kubectl
```

> Linux

和 macOS 系统上安装一样，只是下载地址不一样：

```
curl -LO https://storage.googleapis.com/kubernetes-release/release/{version}/bin/linux/amd64/kubectl
```

## 3、使用 Google Cloud SDK 安装 (Download as part of the Google Cloud SDK)

kubectl 可以作为 Google Cloud SDK 的一部分被安装。

先安装 [Google Cloud SDK](https://cloud.google.com/sdk/)，然后执行以下命令安装 kubectl：

```
gcloud components install kubectl
```

安装完成后运行 `kubectl version` 命令验证安装是否成功

## 4、配置 shell 终端自动补齐命令 (Enabling shell autocompletion) 

kubectl包括自动完成支持，可以节省大量的输入！

完成脚本本身由kubectl生成，因此您通常只需要从配置文件中调用它。

这里提供了常见的例子。有关更多详细信息，请参阅 `kubectl completion -h`

##### 4.1 On Linux, using bash

在CentOS Linux上，您可能需要安装默认情况下未安装的 bash-completion 软件包: `yum install bash-completion -y`

添加 kubectl 自动补全到当前 shell 窗口，请运行：`source <(kubectl completion bash)`

添加 kubectl 自动补全到 profile，使得以后打开 shell 窗口自动加载：`echo "source <(kubectl completion bash)" >> ~/.bashrc
`

##### 4.2 On macOS, using bash

在 macOS上，您需要先通过 Homebrew 安装 bash-completion 支持：

```
## If running Bash 3.2 included with macOS
brew install bash-completion
## or, if running Bash 4.1+
brew install bash-completion@2
```

##### 4.3 Using Zsh
