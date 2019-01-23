VirtualBox 官方下载地址：https://www.virtualbox.org/wiki/Linux_Downloads

--

![](https://lollipop.xiaosongfu.com/blog/201901/virualbox.png)

右键单击 ` Oracle Linux 7 / Red Hat Enterprise Linux 7 / CentOS 7` 并选择 `赋值连接地址`，然后开始下载：

```
[root@DockerApp ~]# wget https://download.virtualbox.org/virtualbox/6.0.0/VirtualBox-6.0-6.0.0_127566_el7-1.x86_64.rpm
```

最后安装：

```
rpm -i VirtualBox-6.0-6.0.0_127566_el7-1.x86_64.rpm
```

报错了：

```
[root@DockerApp ~]# rpm -i VirtualBox-6.0-6.0.0_127566_el7-1.x86_64.rpm
warning: ./VirtualBox-6.0-6.0.0_127566_el7-1.x86_64.rpm: Header V4 DSA/SHA1 Signature, key ID 98ab5139: NOKEY
error: Failed dependencies:
	libGL.so.1()(64bit) is needed by VirtualBox-6.0-6.0.0_127566_el7-1.x86_64
	libSDL-1.2.so.0()(64bit) is needed by VirtualBox-6.0-6.0.0_127566_el7-1.x86_64
	libXcursor.so.1()(64bit) is needed by VirtualBox-6.0-6.0.0_127566_el7-1.x86_64
	libXmu.so.6()(64bit) is needed by VirtualBox-6.0-6.0.0_127566_el7-1.x86_64
	libXt.so.6()(64bit) is needed by VirtualBox-6.0-6.0.0_127566_el7-1.x86_64
	libopus.so.0()(64bit) is needed by VirtualBox-6.0-6.0.0_127566_el7-1.x86_64
	libvpx.so.1()(64bit) is needed by VirtualBox-6.0-6.0.0_127566_el7-1.x86_64
[root@DockerApp ~]#
```

解决方法：

```
yum install libGL libXcursor libXmu libXt libvpx
```

> https://centos.pkgs.org/7/centos-x86_64/opus-1.0.2-6.el7.x86_64.rpm.html

```
wget http://mirror.centos.org/centos/7/os/x86_64/Packages/opus-1.0.2-6.el7.x86_64.rpm
rpm -i opus-1.0.2-6.el7.x86_64.rpm
```

> http://www.libsdl.org/download-1.2.php

```
wget http://www.libsdl.org/release/SDL-1.2.15-1.x86_64.rpm
rpm -i SDL-1.2.15-1.x86_64.rpm
```

最后再执行:

```
[root@DockerApp ~]# rpm -i ./VirtualBox-6.0-6.0.0_127566_el7-1.x86_64.rpm
warning: ./VirtualBox-6.0-6.0.0_127566_el7-1.x86_64.rpm: Header V4 DSA/SHA1 Signature, key ID 98ab5139: NOKEY

Creating group 'vboxusers'. VM users must be member of that group!

This system is currently not set up to build kernel modules.
Please install the gcc make perl packages from your distribution.
Please install the Linux kernel "header" files matching the current kernel
for adding new hardware support to the system.
The distribution packages containing the headers are probably:
    kernel-devel kernel-devel-3.10.0-693.21.1.el7.x86_64
This system is currently not set up to build kernel modules.
Please install the gcc make perl packages from your distribution.
Please install the Linux kernel "header" files matching the current kernel
for adding new hardware support to the system.
The distribution packages containing the headers are probably:
    kernel-devel kernel-devel-3.10.0-693.21.1.el7.x86_64

There were problems setting up VirtualBox.  To re-start the set-up process, run
  /sbin/vboxconfig
as root.
[root@DockerApp ~]#
```