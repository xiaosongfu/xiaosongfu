## 坑1：执行 `kubeadm init` 的命令时需要加上 `--ignore-preflight-errors=NumCPU`

使用1个 CPU 的服务器执行初始化会报错，说只有1个 CPU 不满足 Kubernetes 至少需要2个 CPU 的需求。加上这个参数就可以了。


## 坑2：执行 `kubeadm init` 的命令时，`--apiserver-advertise-address` 需要使用内网 IP 而不能是公网 IP

首先使用公网 IP 试一试：

```
[root@10-254-218-236 dc2-user]# kubeadm init \
>     --apiserver-advertise-address=116.85.50.158 \
>     --image-repository registry.aliyuncs.com/google_containers \
>     --kubernetes-version v1.13.3 \
>     --pod-network-cidr=10.244.0.0/16 \
>     --ignore-preflight-errors=NumCPU
[init] Using Kubernetes version: v1.13.3
[preflight] Running pre-flight checks
	[WARNING NumCPU]: the number of available CPUs 1 is less than the required 2
[preflight] Pulling images required for setting up a Kubernetes cluster
[preflight] This might take a minute or two, depending on the speed of your internet connection
[preflight] You can also perform this action in beforehand using 'kubeadm config images pull'
[kubelet-start] Writing kubelet environment file with flags to file "/var/lib/kubelet/kubeadm-flags.env"
[kubelet-start] Writing kubelet configuration to file "/var/lib/kubelet/config.yaml"
[kubelet-start] Activating the kubelet service
[certs] Using certificateDir folder "/etc/kubernetes/pki"
[certs] Generating "ca" certificate and key
[certs] Generating "apiserver" certificate and key
[certs] apiserver serving cert is signed for DNS names [10-254-218-236 kubernetes kubernetes.default kubernetes.default.svc kubernetes.default.svc.cluster.local] and IPs [10.96.0.1 116.85.50.158]
[certs] Generating "apiserver-kubelet-client" certificate and key
[certs] Generating "front-proxy-ca" certificate and key
[certs] Generating "front-proxy-client" certificate and key
[certs] Generating "etcd/ca" certificate and key
[certs] Generating "etcd/peer" certificate and key
[certs] etcd/peer serving cert is signed for DNS names [10-254-218-236 localhost] and IPs [116.85.50.158 127.0.0.1 ::1]
[certs] Generating "etcd/healthcheck-client" certificate and key
[certs] Generating "etcd/server" certificate and key
[certs] etcd/server serving cert is signed for DNS names [10-254-218-236 localhost] and IPs [116.85.50.158 127.0.0.1 ::1]
[certs] Generating "apiserver-etcd-client" certificate and key
[certs] Generating "sa" key and public key
[kubeconfig] Using kubeconfig folder "/etc/kubernetes"
[kubeconfig] Writing "admin.conf" kubeconfig file
[kubeconfig] Writing "kubelet.conf" kubeconfig file
[kubeconfig] Writing "controller-manager.conf" kubeconfig file
[kubeconfig] Writing "scheduler.conf" kubeconfig file
[control-plane] Using manifest folder "/etc/kubernetes/manifests"
[control-plane] Creating static Pod manifest for "kube-apiserver"
[control-plane] Creating static Pod manifest for "kube-controller-manager"
[control-plane] Creating static Pod manifest for "kube-scheduler"
[etcd] Creating static Pod manifest for local etcd in "/etc/kubernetes/manifests"
[wait-control-plane] Waiting for the kubelet to boot up the control plane as static Pods from directory "/etc/kubernetes/manifests". This can take up to 4m0s
[kubelet-check] Initial timeout of 40s passed.

Unfortunately, an error has occurred:
	timed out waiting for the condition

This error is likely caused by:
	- The kubelet is not running
	- The kubelet is unhealthy due to a misconfiguration of the node in some way (required cgroups disabled)

If you are on a systemd-powered system, you can try to troubleshoot the error with the following commands:
	- 'systemctl status kubelet'
	- 'journalctl -xeu kubelet'

Additionally, a control plane component may have crashed or exited when started by the container runtime.
To troubleshoot, list all containers using your preferred container runtimes CLI, e.g. docker.
Here is one example how you may list all Kubernetes containers running in docker:
	- 'docker ps -a | grep kube | grep -v pause'
	Once you have found the failing container, you can inspect its logs with:
	- 'docker logs CONTAINERID'
error execution phase wait-control-plane: couldn't initialize a Kubernetes cluster
```

发现初始化失败了，根据提示执行 `docker ps -a | grep kube | grep -v pause` 命令：

```
[root@10-254-218-236 dc2-user]# docker ps -a | grep kube | grep -v pause
7add4f83d1ac        3cab8e1b9802                                        "etcd --advertise-cl…"   57 seconds ago       Exited (1) 56 seconds ago                             k8s_etcd_etcd-10-254-218-236_kube-system_2c1f6716ac73687f680c43862726c299_6
463b757ae512        fe242e556a99                                        "kube-apiserver --au…"   About a minute ago   Exited (255) About a minute ago                       k8s_kube-apiserver_kube-apiserver-10-254-218-236_kube-system_e8ba1a09856348b10be9cc939113c3f0_5
79b20ef56a02        3a6f709e97a0                                        "kube-scheduler --ad…"   6 minutes ago        Up 6 minutes                                          k8s_kube-scheduler_kube-scheduler-10-254-218-236_kube-system_20d414b4ad6b736c6622187c49106e1b_0
ccbf5d47a921        0482f6400933                                        "kube-controller-man…"   6 minutes ago        Up 6 minutes                                          k8s_kube-controller-manager_kube-controller-manager-10-254-218-236_kube-system_d95d0109b2cbd60df7a0e1da069217ab_0
```

发现 etcd 和 kube-apiserver 都没有启动成功。其实 `kubeadm init` 初始化失败的原因就是因为是 etcd 没有启动成功，导致 kube-apiserver 因为连接不上 etcd 而启动失败。

我们先看一下 etcd 容器的日志

```
[root@10-254-218-236 dc2-user]# docker logs 7add4f83d1ac
2019-02-21 08:55:41.119901 I | etcdmain: etcd Version: 3.2.24
2019-02-21 08:55:41.119957 I | etcdmain: Git SHA: 420a45226
2019-02-21 08:55:41.119962 I | etcdmain: Go Version: go1.8.7
2019-02-21 08:55:41.119966 I | etcdmain: Go OS/Arch: linux/amd64
2019-02-21 08:55:41.119970 I | etcdmain: setting maximum number of CPUs to 1, total number of available CPUs is 1
2019-02-21 08:55:41.120033 I | embed: peerTLS: cert = /etc/kubernetes/pki/etcd/peer.crt, key = /etc/kubernetes/pki/etcd/peer.key, ca = , trusted-ca = /etc/kubernetes/pki/etcd/ca.crt, client-cert-auth = true
2019-02-21 08:55:41.120095 C | etcdmain: listen tcp 116.85.50.158:2380: bind: cannot assign requested address
``` 

报错了，错误信息如下：

```
etcdmain: listen tcp 116.85.50.158:2380: bind: cannot assign requested address
```

应该是因为滴滴云的原因。
解决方案：在执行 `kubeadm init` 的命令时，`--apiserver-advertise-address` 需要使用内网 IP 而不能是公网 IP，换成内网 IP 就可以初始化成功。