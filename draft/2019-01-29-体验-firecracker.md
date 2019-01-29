## 1. 要求

Firecracker 在 [KVM](https://www.linux-kvm.org/) 上构建并且需要 `/dev/kvm` 的读/写权限。登录一个终端中的主机，然后设置该访问权限：

```
sudo chmod 777 /dev/kvm
```

## 2. 下载 Firecracker 可执行文件

下载 Firecracker：

```
# 获取最新的 release 版本号
export RELEASE=$(curl -s https://api.github.com/repos/firecracker-microvm/firecracker/releases/latest | grep tag_name | cut -d '"' -f 4)

# 开始下载最新版本
curl -LOJ https://github.com/firecracker-microvm/firecracker/releases/download/${RELEASE}/firecracker-${RELEASE}
```

重命名为 firecracker 并添加可执行权限：

```
mv firecracker-${RELEASE} firecracker

chmod +x firecracker
```

## 3. 启动

接下来，您将需要一个未压缩的 Linux 内核二进制文件和一个 ext4 文件系统映像（用作rootfs）。您可以使用我们S3 存储桶中的 microVM 映像：[kernel](https://s3.amazonaws.com/spec.ccfc.min/img/hello/kernel/hello-vmlinux.bin) 和 [rootfs](https://s3.amazonaws.com/spec.ccfc.min/img/hello/fsfiles/hello-rootfs.ext4)。

现在，让我们打开两个 shell 窗口：一个用于运行 Firecracker，另一个用于控制它（通过写入 API socket）。

#### 在第一个 shell 窗口：

确保Firecracker可以创建其API套接字：

```
rm -f /tmp/firecracker.socket
```

启动 Firecracker：

```
firecracker --api-sock /tmp/firecracker.sock
```

#### 在第二个 shell 窗口：

* 如果没有 kernel 和 rootfs，先下载他们：

```
curl -fsSL -o hello-vmlinux.bin https://s3.amazonaws.com/spec.ccfc.min/img/hello/kernel/hello-vmlinux.bin
curl -fsSL -o hello-rootfs.ext4 https://s3.amazonaws.com/spec.ccfc.min/img/hello/fsfiles/hello-rootfs.ext4
```

* 设置 kernel：

```
curl --unix-socket /tmp/firecracker.socket -i \
    -X PUT 'http://localhost/boot-source'   \
    -H 'Accept: application/json'           \
    -H 'Content-Type: application/json'     \
    -d '{
        "kernel_image_path": "./hello-vmlinux.bin",
        "boot_args": "console=ttyS0 reboot=k panic=1 pci=off"
    }'
```

* 设置 rootfs：

```
curl --unix-socket /tmp/firecracker.socket -i \
    -X PUT 'http://localhost/drives/rootfs' \
    -H 'Accept: application/json'           \
    -H 'Content-Type: application/json'     \
    -d '{
        "drive_id": "rootfs",
        "path_on_host": "./hello-rootfs.ext4",
        "is_root_device": true,
        "is_read_only": false
    }'
```

* 启动客户机：

```
curl --unix-socket /tmp/firecracker.socket -i \
    -X PUT 'http://localhost/actions'       \
    -H  'Accept: application/json'          \
    -H  'Content-Type: application/json'    \
    -d '{
        "action_type": "InstanceStart"
     }'
```

回到第一个 shell，现在应该看到一个串行 TTY，提示您登录到来宾计算机。如果您使用了我们的 hello-rootfs.ext4 映像，则可以使用密码 root 以 root 身份登录。

在 guest 虚拟机内发出 reboot 命令将正常关闭 Firecracker。这是因为，由于 microVM 不是为重新启动而设计的，而且 Firecracker 目前没有实现客户电源管理，我们将键盘复位操作用作关闭开关。

## 4. 其他

##### 4.1 查看配置：

```
curl --unix-socket /tmp/firecracker.sock "http://localhost/machine-config"
```

##### 4.2 修改配置：

默认的 microVM 将具有1个 vCPU 和128个 MiB RAM。如果您希望自定义（例如，2个 vCPU和1024MiB RAM），则可以在发出 InstanceStart 调用之前通过此 API 命令执行此操作：

```
curl --unix-socket /tmp/firecracker.socket -i  \
    -X PUT 'http://localhost/machine-config' \
    -H 'Accept: application/json'            \
    -H 'Content-Type: application/json'      \
    -d '{
        "vcpu_count": 2,
        "mem_size_mib": 1024
    }'
```

---

参考：

1. [Getting Started with Firecracker](https://github.com/firecracker-microvm/firecracker/blob/master/docs/getting-started.md)
2. [宣布推出 Firecracker 开源技术：适用于无服务器计算的安全、快速的 microVM](https://aws.amazon.com/cn/blogs/china/firecracker-open-source-secure-fast-microvm-serverless/)
3. [firecracker releases](https://github.com/firecracker-microvm/firecracker/releases)