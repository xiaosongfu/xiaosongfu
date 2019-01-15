---
title: "Centos 7 使用 kubeadm 安装 Kubernetes 集群"
date: 2018-08-17T11:52:24+08:00
tags: []
categories: []
---

## 目录
>  
1、安装 Docker
2、安装 kubeadm, kubelet 和 kubectl
3、用 kubeadm 创建 Cluster
4、配置 kubectl
5、安装 pod 网络附加组件
6、添加 Node 节点
7、重置安装
8、附录和遇到的问题

* 本文整理自 [Installing kubeadm](https://kubernetes.cn/docs/setup/independent/install-kubeadm/#installing-kubeadm-kubelet-and-kubectl)

> 本文以 Centos 7 为例。

### 1、安装 Docker

Centos 系统上安装：[Get Docker CE for CentOS](https://docs.docker.com/install/linux/docker-ce/centos/)

使用如下命令安装 `17.03` 版本的 Docker：

```
[root@VM_0_3_centos ~]# yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2

[root@VM_0_3_centos ~]# yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo

[root@VM_0_3_centos ~]# yum list docker-ce --showduplicates | sort -r
已加载插件：fastestmirror, langpacks
可安装的软件包
Loading mirror speeds from cached hostfile
docker-ce.x86_64            18.06.0.ce-3.el7                    docker-ce-stable
docker-ce.x86_64            18.03.1.ce-1.el7.centos             docker-ce-stable
docker-ce.x86_64            18.03.0.ce-1.el7.centos             docker-ce-stable
docker-ce.x86_64            17.12.1.ce-1.el7.centos             docker-ce-stable
docker-ce.x86_64            17.12.0.ce-1.el7.centos             docker-ce-stable
docker-ce.x86_64            17.09.1.ce-1.el7.centos             docker-ce-stable
docker-ce.x86_64            17.09.0.ce-1.el7.centos             docker-ce-stable
docker-ce.x86_64            17.06.2.ce-1.el7.centos             docker-ce-stable
docker-ce.x86_64            17.06.1.ce-1.el7.centos             docker-ce-stable
docker-ce.x86_64            17.06.0.ce-1.el7.centos             docker-ce-stable
docker-ce.x86_64            17.03.2.ce-1.el7.centos             docker-ce-stable
docker-ce.x86_64            17.03.1.ce-1.el7.centos             docker-ce-stable
docker-ce.x86_64            17.03.0.ce-1.el7.centos             docker-ce-stable

[root@VM_0_3_centos ~]# yum -y install docker-ce-17.03.0.ce
```

启动 docker，并设置开机自启动：

```
[root@VM_0_3_centos ~]# systemctl enable docker && systemctl start docker
```

### 2、安装 kubeadm, kubelet 和 kubectl

需要在所有节点上安装 kubeadm, kubelet 和 kubectl。

* kubeadm 用于初始化 Cluster。
* kubelet 运行在 Cluster 所有节点上，负责启动 Pod 和容器。
* kubectl 是 Kubernetes 命令行工具。通过 kubectl 可以部署和管理应用，查看各种资源，创建、删除和更新各种组件。

```
[root@VM_0_3_centos ~]# cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
exclude=kube*
EOF
```

Set SELinux in permissive mode (effectively disabling it)

```
[root@VM_0_3_centos ~]# setenforce 0
[root@VM_0_3_centos ~]# sed -i 's/^SELINUX=enforcing$/SELINUX=permissive/' /etc/selinux/config
```

```
[root@VM_0_3_centos ~]# yum install -y kubelet kubeadm kubectl --disableexcludes=kubernetes
已加载插件：fastestmirror, langpacks
Repository base is listed more than once in the configuration
Repository updates is listed more than once in the configuration
Repository extras is listed more than once in the configuration
Repository centosplus is listed more than once in the configuration
Loading mirror speeds from cached hostfile
 * epel: mirror01.idc.hinet.net
kubernetes/signature                                                                                                      |  454 B  00:00:00
从 https://packages.cloud.google.com/yum/doc/yum-key.gpg 检索密钥
导入 GPG key 0xA7317B0F:
 用户ID     : "Google Cloud Packages Automatic Signing Key <gc-team@google.com>"
 指纹       : d0bc 747f d8ca f711 7500 d6fa 3746 c208 a731 7b0f
 来自       : https://packages.cloud.google.com/yum/doc/yum-key.gpg
从 https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg 检索密钥
kubernetes/signature                                                                                                      | 1.4 kB  00:00:00 !!!
kubernetes/primary                                                                                                        |  34 kB  00:00:01
kubernetes                                                                                                                               243/243
正在解决依赖关系
--> 正在检查事务
---> 软件包 kubeadm.x86_64.0.1.11.2-0 将被 安装
--> 正在处理依赖关系 kubernetes-cni >= 0.6.0，它被软件包 kubeadm-1.11.2-0.x86_64 需要
--> 正在处理依赖关系 cri-tools >= 1.11.0，它被软件包 kubeadm-1.11.2-0.x86_64 需要
---> 软件包 kubectl.x86_64.0.1.11.2-0 将被 安装
---> 软件包 kubelet.x86_64.0.1.11.2-0 将被 安装
--> 正在检查事务
---> 软件包 cri-tools.x86_64.0.1.11.0-0 将被 安装
---> 软件包 kubernetes-cni.x86_64.0.0.6.0-0 将被 安装
--> 解决依赖关系完成

依赖关系解决

=================================================================================================================================================
 Package                                架构                           版本                             源                                  大小
=================================================================================================================================================
正在安装:
 kubeadm                                x86_64                         1.11.2-0                         kubernetes                         7.5 M
 kubectl                                x86_64                         1.11.2-0                         kubernetes                         7.5 M
 kubelet                                x86_64                         1.11.2-0                         kubernetes                          18 M
为依赖而安装:
 cri-tools                              x86_64                         1.11.0-0                         kubernetes                         4.2 M
 kubernetes-cni                         x86_64                         0.6.0-0                          kubernetes                         8.6 M

事务概要
=================================================================================================================================================
安装  3 软件包 (+2 依赖软件包)

总下载量：46 M
安装大小：228 M
Downloading packages:
警告：/var/cache/yum/x86_64/7/kubernetes/packages/e253c692a017b164ebb9ad1b6537ff8afd93c35e9ebc340a52c5bd42425c0760-cri-tools-1.11.0-0.x86_64.rpm: 头V4 RSA/SHA512 Signature, 密钥 ID 3e1ba8d5: NOKEY
e253c692a017b164ebb9ad1b6537ff8afd93c35e9ebc340a52c5bd42425c0760-cri-tools-1.11.0-0.x86_64.rpm 的公钥尚未安装
(1/5): e253c692a017b164ebb9ad1b6537ff8afd93c35e9ebc340a52c5bd42425c0760-cri-tools-1.11.0-0.x86_64.rpm                     | 4.2 MB  00:00:03
(2/5): a554c1728ecf79871b4d3e0fc797568e53149f4ed7ec7e437c949a02f197a1ab-kubectl-1.11.2-0.x86_64.rpm                       | 7.5 MB  00:00:02
(3/5): 2cb76e20a5cf7c7006b5e13c2f53838eda7a3c1283773646d7b2ec4dc63d417a-kubeadm-1.11.2-0.x86_64.rpm                       | 7.5 MB  00:00:09
(4/5): fe33057ffe95bfae65e2f269e1b05e99308853176e24a4d027bc082b471a07c0-kubernetes-cni-0.6.0-0.x86_64.rpm                 | 8.6 MB  00:00:01
(5/5): 518f6ba7535c69a4fa3fc7b40e370c4c4821ba9231fc25dafa556f5b4554b001-kubelet-1.11.2-0.x86_64.rpm                       |  18 MB  00:00:15
-------------------------------------------------------------------------------------------------------------------------------------------------
总计                                                                                                             2.2 MB/s |  46 MB  00:00:21
从 https://packages.cloud.google.com/yum/doc/yum-key.gpg 检索密钥
导入 GPG key 0xA7317B0F:
 用户ID     : "Google Cloud Packages Automatic Signing Key <gc-team@google.com>"
 指纹       : d0bc 747f d8ca f711 7500 d6fa 3746 c208 a731 7b0f
 来自       : https://packages.cloud.google.com/yum/doc/yum-key.gpg
从 https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg 检索密钥
导入 GPG key 0x3E1BA8D5:
 用户ID     : "Google Cloud Packages RPM Signing Key <gc-team@google.com>"
 指纹       : 3749 e1ba 95a8 6ce0 5454 6ed2 f09c 394c 3e1b a8d5
 来自       : https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  正在安装    : kubelet-1.11.2-0.x86_64                                                                                                      1/5
  正在安装    : kubernetes-cni-0.6.0-0.x86_64                                                                                                2/5
  正在安装    : kubectl-1.11.2-0.x86_64                                                                                                      3/5
  正在安装    : cri-tools-1.11.0-0.x86_64                                                                                                    4/5
  正在安装    : kubeadm-1.11.2-0.x86_64                                                                                                      5/5
  验证中      : cri-tools-1.11.0-0.x86_64                                                                                                    1/5
  验证中      : kubectl-1.11.2-0.x86_64                                                                                                      2/5
  验证中      : kubernetes-cni-0.6.0-0.x86_64                                                                                                3/5
  验证中      : kubeadm-1.11.2-0.x86_64                                                                                                      4/5
  验证中      : kubelet-1.11.2-0.x86_64                                                                                                      5/5

已安装:
  kubeadm.x86_64 0:1.11.2-0                       kubectl.x86_64 0:1.11.2-0                       kubelet.x86_64 0:1.11.2-0

作为依赖被安装:
  cri-tools.x86_64 0:1.11.0-0                                           kubernetes-cni.x86_64 0:0.6.0-0

完毕！
[root@JVM_0_3_centos ~]#
```

启动 kubelet，并设置开机自启动：

```
[root@VM_0_3_centos ~]# systemctl enable kubelet && systemctl start kubelet
```

注意：

* `setenforce 0` 必须执行
* RHEL/CentOS 7 系统上可能需要设置 `net.bridge.bridge-nf-call-iptables` 为 1，在 `sysctl` 配置里，如：

```
cat <<EOF >  /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF
sysctl --system
```

> 我在部署的过程中，node 节点为 Centos 7，必须这些了这个才可以 join 成功。

### 3、用 kubeadm 创建 Cluster

可以使用 `kubeadm config images list` 命令查看本次安装会用到的 k8s.gcr.io 仓库里的镜像的版本号，如：

```
[root@VM_0_3_centos ~]# kubeadm config images list
k8s.gcr.io/kube-apiserver-amd64:v1.11.2
k8s.gcr.io/kube-controller-manager-amd64:v1.11.2
k8s.gcr.io/kube-scheduler-amd64:v1.11.2
k8s.gcr.io/kube-proxy-amd64:v1.11.2
k8s.gcr.io/pause:3.1
k8s.gcr.io/etcd-amd64:3.2.18
k8s.gcr.io/coredns:1.1.3
[root@VM_0_3_centos ~]#
```

在执行 `kubeadm init` 命令之前可以执行 `kubeadm config images pull` 命令把所有等下需要的 k8s.gcr.io 仓库里的镜像拉取下来。

> 建议先拉取镜像

`kubeadm config images pull` 命令的运行结果：

```
[root@VM_0_3_centos ~]# kubeadm config images pull
[config/images] Pulled k8s.gcr.io/kube-apiserver-amd64:v1.11.2
[config/images] Pulled k8s.gcr.io/kube-controller-manager-amd64:v1.11.2
[config/images] Pulled k8s.gcr.io/kube-scheduler-amd64:v1.11.2
[config/images] Pulled k8s.gcr.io/kube-proxy-amd64:v1.11.2
[config/images] Pulled k8s.gcr.io/pause:3.1
[config/images] Pulled k8s.gcr.io/etcd-amd64:3.2.18
[config/images] Pulled k8s.gcr.io/coredns:1.1.3
[root@VM_0_3_centos ~]#
```

> 需要为 Docker 设置好代理后，才可以下载 k8s.gcr.io 仓库下的镜像。

执行 init 命令初始化：

```
[root@VM_0_3_centos ~]# kubeadm init --apiserver-advertise-address 132.232.5.36 --pod-network-cidr=10.244.0.0/16
[init] using Kubernetes version: v1.11.2
[preflight] running pre-flight checks
	[WARNING HTTPProxy]: Connection to "https://132.232.5.36" uses proxy "http://127.0.0.1:8118". If that is not intended, adjust your proxy settings
	[WARNING HTTPProxyCIDR]: connection to "10.96.0.0/12" uses proxy "http://127.0.0.1:8118". This may lead to malfunctional cluster setup. Make sure that Pod and Services IP ranges specified correctly as exceptions in proxy configuration
	[WARNING HTTPProxyCIDR]: connection to "10.244.0.0/16" uses proxy "http://127.0.0.1:8118". This may lead to malfunctional cluster setup. Make sure that Pod and Services IP ranges specified correctly as exceptions in proxy configuration
I0817 16:38:24.065948    6494 kernel_validator.go:81] Validating kernel version
I0817 16:38:24.066023    6494 kernel_validator.go:96] Validating kernel config
	[WARNING SystemVerification]: docker version is greater than the most recently validated version. Docker version: 18.06.0-ce. Max validated version: 17.03
	[WARNING Service-Kubelet]: kubelet service is not enabled, please run 'systemctl enable kubelet.service'
[preflight/images] Pulling images required for setting up a Kubernetes cluster
[preflight/images] This might take a minute or two, depending on the speed of your internet connection
[preflight/images] You can also perform this action in beforehand using 'kubeadm config images pull'
[kubelet] Writing kubelet environment file with flags to file "/var/lib/kubelet/kubeadm-flags.env"
[kubelet] Writing kubelet configuration to file "/var/lib/kubelet/config.yaml"
[preflight] Activating the kubelet service
[certificates] Generated ca certificate and key.
[certificates] Generated apiserver certificate and key.
[certificates] apiserver serving cert is signed for DNS names [master kubernetes kubernetes.default kubernetes.default.svc kubernetes.default.svc.cluster.local] and IPs [10.96.0.1 132.232.5.36]
[certificates] Generated apiserver-kubelet-client certificate and key.
[certificates] Generated sa key and public key.
[certificates] Generated front-proxy-ca certificate and key.
[certificates] Generated front-proxy-client certificate and key.
[certificates] Generated etcd/ca certificate and key.
[certificates] Generated etcd/server certificate and key.
[certificates] etcd/server serving cert is signed for DNS names [master localhost] and IPs [127.0.0.1 ::1]
[certificates] Generated etcd/peer certificate and key.
[certificates] etcd/peer serving cert is signed for DNS names [master localhost] and IPs [132.232.5.36 127.0.0.1 ::1]
[certificates] Generated etcd/healthcheck-client certificate and key.
[certificates] Generated apiserver-etcd-client certificate and key.
[certificates] valid certificates and keys now exist in "/etc/kubernetes/pki"
[kubeconfig] Wrote KubeConfig file to disk: "/etc/kubernetes/admin.conf"
[kubeconfig] Wrote KubeConfig file to disk: "/etc/kubernetes/kubelet.conf"
[kubeconfig] Wrote KubeConfig file to disk: "/etc/kubernetes/controller-manager.conf"
[kubeconfig] Wrote KubeConfig file to disk: "/etc/kubernetes/scheduler.conf"
[controlplane] wrote Static Pod manifest for component kube-apiserver to "/etc/kubernetes/manifests/kube-apiserver.yaml"
[controlplane] wrote Static Pod manifest for component kube-controller-manager to "/etc/kubernetes/manifests/kube-controller-manager.yaml"
[controlplane] wrote Static Pod manifest for component kube-scheduler to "/etc/kubernetes/manifests/kube-scheduler.yaml"
[etcd] Wrote Static Pod manifest for a local etcd instance to "/etc/kubernetes/manifests/etcd.yaml"
[init] waiting for the kubelet to boot up the control plane as Static Pods from directory "/etc/kubernetes/manifests"
[init] this might take a minute or longer if the control plane images have to be pulled
[apiclient] All control plane components are healthy after 45.953916 seconds
[uploadconfig] storing the configuration used in ConfigMap "kubeadm-config" in the "kube-system" Namespace
[kubelet] Creating a ConfigMap "kubelet-config-1.11" in namespace kube-system with the configuration for the kubelets in the cluster
[markmaster] Marking the node master as master by adding the label "node-role.kubernetes.io/master=''"
[markmaster] Marking the node master as master by adding the taints [node-role.kubernetes.io/master:NoSchedule]
[patchnode] Uploading the CRI Socket information "/var/run/dockershim.sock" to the Node API object "master" as an annotation
[bootstraptoken] using token: nk1bb2.oj4jub5x75k4dbj7
[bootstraptoken] configured RBAC rules to allow Node Bootstrap tokens to post CSRs in order for nodes to get long term certificate credentials
[bootstraptoken] configured RBAC rules to allow the csrapprover controller automatically approve CSRs from a Node Bootstrap Token
[bootstraptoken] configured RBAC rules to allow certificate rotation for all node client certificates in the cluster
[bootstraptoken] creating the "cluster-info" ConfigMap in the "kube-public" namespace
[addons] Applied essential addon: CoreDNS
[addons] Applied essential addon: kube-proxy

Your Kubernetes master has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

You can now join any number of machines by running the following on each node
as root:

  kubeadm join 132.232.5.36:6443 --token nk1bb2.oj4jub5x75k4dbj7 --discovery-token-ca-cert-hash sha256:fe572b7b882d1692b7f42befe19a54726935c0707306275271ce3797431b9ce2

[root@VM_0_3_centos ~]#
```

### 4、配置 kubectl

kubectl 是管理 Kubernetes Cluster 的命令行工具，前面我们已经在所有的节点安装了 kubectl。Master 初始化完成后需要做一些配置工作，然后 kubectl 就能使用了。

推荐用普通用户执行 kubectl 命令，需要做如下配置：

```
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

> 我在部署过程中执行后两个命令报错了，参照[解决linux下sudo更改文件权限报错xxxis not in the sudoers file. This incident will be reported.](https://blog.csdn.net/sinat_36118270/article/details/62899093)操作就可以了。

### 5、安装 pod 网络附加组件

要让 Kubernetes Cluster 能够工作，必须安装 Pod 网络，否则 Pod 之间无法通信。

Kubernetes 支持多种网络方案，这里我们使用 flannel。

执行如下命令部署 flannel：

```
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/v0.10.0/Documentation/kube-flannel.yml
```

运行结果：

```
[kube@master ~]$ kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/v0.10.0/Documentation/kube-flannel.yml
clusterrole.rbac.authorization.k8s.io/flannel created
clusterrolebinding.rbac.authorization.k8s.io/flannel created
serviceaccount/flannel created
configmap/kube-flannel-cfg created
daemonset.extensions/kube-flannel-ds created
[kube@master ~]$
```

pod network 安装完成后，运行命令 `kubectl get pods --all-namespaces` 检查 CoreDNS pod 是否在运行，确认 CoreDNS pod 在正常运行后就可以添加 Node 节点了：

```
[kube@master ~]$kubectl get pods --all-namespaces
NAMESPACE     NAME                                   READY     STATUS    RESTARTS   AGE
kube-system   coredns-78fcdf6894-mvjjg               1/1       Running   0          1h
kube-system   coredns-78fcdf6894-mxsth               1/1       Running   0          1h
kube-system   etcd-master                            1/1       Running   0          1h
kube-system   kube-apiserver-master                  1/1       Running   0          1h
kube-system   kube-controller-manager-master         1/1       Running   1          1h
kube-system   kube-flannel-ds-nkdvs                  1/1       Running   2          59m
kube-system   kube-flannel-ds-vw6m9                  1/1       Running   0          1h
kube-system   kube-proxy-794g5                       1/1       Running   0          1h
kube-system   kube-proxy-bkdng                       1/1       Running   0          59m
kube-system   kube-scheduler-master                  1/1       Running   0          1h
```

> 注：默认情况下，处于安全考虑不会调度 pod 到 master 节点，如果需要调度 pod 到 master 节点，可以执行如下命令：

```
kubectl taint nodes --all node-role.kubernetes.io/master-
```

### 6、添加 Node 节点

在 node 节点上执行如下命令加入集群：

```
[root@JD ~]# kubeadm join 132.232.5.36:6443 --token nk1bb2.oj4jub5x75k4dbj7 --discovery-token-ca-cert-hash sha256:fe572b7b882d1692b7f42befe19a54726935c0707306275271ce3797431b9ce2
[preflight] running pre-flight checks
	[WARNING RequiredIPVSKernelModulesAvailable]: the IPVS proxier will not be used, because the following required kernel modules are not loaded: [ip_vs_rr ip_vs_wrr ip_vs_sh ip_vs] or no builtin kernel ipvs support: map[ip_vs:{} ip_vs_rr:{} ip_vs_wrr:{} ip_vs_sh:{} nf_conntrack_ipv4:{}]
you can solve this problem with following methods:
 1. Run 'modprobe -- ' to load missing kernel modules;
2. Provide the missing builtin kernel ipvs support

I0817 17:02:46.617497   28507 kernel_validator.go:81] Validating kernel version
I0817 17:02:46.617581   28507 kernel_validator.go:96] Validating kernel config
	[WARNING SystemVerification]: docker version is greater than the most recently validated version. Docker version: 18.06.0-ce. Max validated version: 17.03
	[WARNING Hostname]: hostname "jd" could not be reached
	[WARNING Hostname]: hostname "jd" lookup jd on 103.224.222.222:53: no such host
[discovery] Trying to connect to API Server "132.232.5.36:6443"
[discovery] Created cluster-info discovery client, requesting info from "https://132.232.5.36:6443"
[discovery] Requesting info from "https://132.232.5.36:6443" again to validate TLS against the pinned public key
[discovery] Cluster info signature and contents are valid and TLS certificate validates against pinned roots, will use API Server "132.232.5.36:6443"
[discovery] Successfully established connection with API Server "132.232.5.36:6443"
[kubelet] Downloading configuration for the kubelet from the "kubelet-config-1.11" ConfigMap in the kube-system namespace
[kubelet] Writing kubelet configuration to file "/var/lib/kubelet/config.yaml"
[kubelet] Writing kubelet environment file with flags to file "/var/lib/kubelet/kubeadm-flags.env"
[preflight] Activating the kubelet service
[tlsbootstrap] Waiting for the kubelet to perform the TLS Bootstrap...
[patchnode] Uploading the CRI Socket information "/var/run/dockershim.sock" to the Node API object "jd" as an annotation

This node has joined the cluster:
* Certificate signing request was sent to master and a response
  was received.
* The Kubelet was informed of the new secure connection details.

Run 'kubectl get nodes' on the master to see this node join the cluster.
[root@JD ~]#
```

按照提示在 master 节点上执行 `kubectl get nodes` 命令查看节点是否加入成功，结果如下：

```
[kube@master ~]$ kubectl get nodes
NAME      STATUS     ROLES     AGE       VERSION
jd        NotReady   <none>    5m        v1.11.2
master    Ready      master    28m       v1.11.2
```

目前 jd 是 NotReady，这是因为每个节点都需要启动若干组件，这些组件都是在 Pod 中运行，需要首先从 google 下载镜像，我们可以通过如下命令查看 Pod 的状态：

```
[kube@master ~]$ kubectl get pods --all-namespaces
NAMESPACE     NAME                             READY     STATUS              RESTARTS   AGE
kube-system   coredns-78fcdf6894-mvjjg         1/1       Running             0          11m
kube-system   coredns-78fcdf6894-mxsth         1/1       Running             0          11m
kube-system   etcd-master                      1/1       Running             0          11m
kube-system   kube-apiserver-master            1/1       Running             0          11m
kube-system   kube-controller-manager-master   1/1       Running             1          11m
kube-system   kube-flannel-ds-nkdvs            0/1       Init:0/1            0          8m
kube-system   kube-flannel-ds-vw6m9            1/1       Running             0          16m
kube-system   kube-proxy-794g5                 1/1       Running             0          30m
kube-system   kube-proxy-bkdng                 0/1       ContainerCreating   0          8m
kube-system   kube-scheduler-master            1/1       Running             0          11m
```

为了节省篇幅，这里只截取命令输出的最后部分。Pending、ContainerCreating、ImagePullBackOff 都表明 Pod 没有就绪，Running 才是就绪状态。我们可以通过 kubectl describe pod <Pod Name> 查看 Pod 具体情况，比如：

```
[kube@master ~]$ kubectl describe pod kube-flannel-ds-nkdvs --namespace=kube-system

......

Events:
  Type     Reason                  Age                From         Message
  ----     ------                  ----               ----         -------
  Warning  FailedCreatePodSandBox  2s (x24 over 10m)  kubelet, jd  Failed create pod sandbox: rpc error: code = Unknown desc = failed pulling image "k8s.gcr.io/pause:3.1": Error response from daemon: Get https://k8s.gcr.io/v2/: net/http: request canceled while waiting for connection (Client.Timeout exceeded while awaiting headers)
```

可以看到 image "k8s.gcr.io/pause:3.1" 下载失败了，如果网络质量不好，我们可以耐心等待，因为 Kubernetes 会重试。当然我们也可以现在用 `docker pull` 命令下载它。

其实可以在 node 节点上提前下好需要用到的镜像，需要两个：`k8s.gcr.io/pause:3.1` 和 `quay.io/coreos/flannel:v0.10.0-amd64`。

最后，所有的 pod 都变成 Running 状态:

```
[kube@master ~]$ kubectl get pods --all-namespaces
NAMESPACE     NAME                             READY     STATUS    RESTARTS   AGE
kube-system   coredns-78fcdf6894-mvjjg         1/1       Running   0          32m
kube-system   coredns-78fcdf6894-mxsth         1/1       Running   0          32m
kube-system   etcd-master                      1/1       Running   0          32m
kube-system   kube-apiserver-master            1/1       Running   0          32m
kube-system   kube-controller-manager-master   1/1       Running   1          32m
kube-system   kube-flannel-ds-nkdvs            1/1       Running   2          29m
kube-system   kube-flannel-ds-vw6m9            1/1       Running   0          36m
kube-system   kube-proxy-794g5                 1/1       Running   0          51m
kube-system   kube-proxy-bkdng                 1/1       Running   0          29m
kube-system   kube-scheduler-master            1/1       Running   0          32m
[kube@master ~]$
```

如果 node 都已经 Ready，Kubernetes Cluster 就创建成功了。

```
[kube@master ~]$ kubectl get nodes
NAME      STATUS    ROLES     AGE       VERSION
jd        Ready     <none>    30m       v1.11.2
master    Ready     master    53m       v1.11.2
[kube@master ~]$
```

最后执行 `kubectl cluster-info` 命令查看集群信息：

```
[kube@master ~]$ kubectl cluster-info
Kubernetes master is running at https://132.232.5.36:6443
KubeDNS is running at https://132.232.5.36:6443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
[kube@master ~]$
```

### 7、重置安装

重置安装 `kubeadm` 的安装工作，首先需要移除 node，运行：

```
kubectl drain <node name> --delete-local-data --force --ignore-daemonsets
kubectl delete node <node name>
```

然后重置 `kubeadm` 的所有安装：

```
kubeadm reset
```

### 8、附录和遇到的问题

##### Docker 设置好代理后，才可以下载 k8s.gcr.io 仓库下的镜像

> 参考文章：https://blog.csdn.net/shida_csdn/article/details/79757793

```
vim /lib/systemd/system/docker.service
```

```
Environment="HTTP_PROXY=http://127.0.0.1:8118/"
Environment="HTTPS_PROXY=http://127.0.0.1:8118/"
```

这里的 HTTP_PROXY 和 HTTPS_PROXY 的地址是 2018-08-17-install-install-sslocal 安装代理后的。

##### kubectl describe pod kube-flannel-ds-nkdvs --namespace=kube-system 详细信息

```
[kube@master ~]$ kubectl describe pod kube-flannel-ds-nkdvs --namespace=kube-system
Name:               kube-flannel-ds-nkdvs
Namespace:          kube-system
Priority:           0
PriorityClassName:  <none>
Node:               jd/192.168.0.3
Start Time:         Fri, 17 Aug 2018 17:02:50 +0800
Labels:             app=flannel
                    controller-revision-hash=911701653
                    pod-template-generation=1
                    tier=node
Annotations:        <none>
Status:             Pending
IP:                 192.168.0.3
Controlled By:      DaemonSet/kube-flannel-ds
Init Containers:
  install-cni:
    Container ID:
    Image:         quay.io/coreos/flannel:v0.10.0-amd64
    Image ID:
    Port:          <none>
    Host Port:     <none>
    Command:
      cp
    Args:
      -f
      /etc/kube-flannel/cni-conf.json
      /etc/cni/net.d/10-flannel.conflist
    State:          Waiting
      Reason:       PodInitializing
    Ready:          False
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /etc/cni/net.d from cni (rw)
      /etc/kube-flannel/ from flannel-cfg (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from flannel-token-q47ns (ro)
Containers:
  kube-flannel:
    Container ID:
    Image:         quay.io/coreos/flannel:v0.10.0-amd64
    Image ID:
    Port:          <none>
    Host Port:     <none>
    Command:
      /opt/bin/flanneld
    Args:
      --ip-masq
      --kube-subnet-mgr
    State:          Waiting
      Reason:       PodInitializing
    Ready:          False
    Restart Count:  0
    Limits:
      cpu:     100m
      memory:  50Mi
    Requests:
      cpu:     100m
      memory:  50Mi
    Environment:
      POD_NAME:       kube-flannel-ds-nkdvs (v1:metadata.name)
      POD_NAMESPACE:  kube-system (v1:metadata.namespace)
    Mounts:
      /etc/kube-flannel/ from flannel-cfg (rw)
      /run from run (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from flannel-token-q47ns (ro)
Conditions:
  Type              Status
  Initialized       False
  Ready             False
  ContainersReady   False
  PodScheduled      True
Volumes:
  run:
    Type:          HostPath (bare host directory volume)
    Path:          /run
    HostPathType:
  cni:
    Type:          HostPath (bare host directory volume)
    Path:          /etc/cni/net.d
    HostPathType:
  flannel-cfg:
    Type:      ConfigMap (a volume populated by a ConfigMap)
    Name:      kube-flannel-cfg
    Optional:  false
  flannel-token-q47ns:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  flannel-token-q47ns
    Optional:    false
QoS Class:       Guaranteed
Node-Selectors:  beta.kubernetes.io/arch=amd64
Tolerations:     node-role.kubernetes.io/master:NoSchedule
                 node.kubernetes.io/disk-pressure:NoSchedule
                 node.kubernetes.io/memory-pressure:NoSchedule
                 node.kubernetes.io/not-ready:NoExecute
                 node.kubernetes.io/unreachable:NoExecute
Events:
  Type     Reason                  Age                From         Message
  ----     ------                  ----               ----         -------
  Warning  FailedCreatePodSandBox  2s (x24 over 10m)  kubelet, jd  Failed create pod sandbox: rpc error: code = Unknown desc = failed pulling image "k8s.gcr.io/pause:3.1": Error response from daemon: Get https://k8s.gcr.io/v2/: net/http: request canceled while waiting for connection (Client.Timeout exceeded while awaiting headers)
```