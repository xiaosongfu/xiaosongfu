# https://hub.docker.com/r/consol/ubuntu-xfce-vnc/

提供一个 docker 镜像，运行它即可得到一个远程桌面服务器，包括：

* Desktop environment Xfce4 or IceWM
* VNC-Server (default VNC port 5901)
* noVNC - HTML5 VNC client (default http port 6901)
* Browsers:
    * Mozilla Firefox
    * Chromium

---

* 提供的操作系统有 Centos 和 Ubuntu；
* 桌面环境有 Xfce4 和 IceWM；
* 还提供 vnc 客户端和浏览器访问方式；
* 里面自带了 Firefox 和 Chromium 浏览器

---

## 安装并通过浏览器连接

安装：

```
# 这4个任远一个就可以了，看自己喜欢
docker run -d -p 5901:5901 -p 6901:6901 consol/ubuntu-xfce-vnc
docker run -d -p 5901:5901 -p 6901:6901 consol/centos-xfce-vnc
docker run -d -p 5901:5901 -p 6901:6901 consol/ubuntu-icewm-vnc
docker run -d -p 5901:5901 -p 6901:6901 consol/centos-icewm-vnc
```

> 5901 (vnc protocol) ： VNC 服务端口
> 6901 (vnc web access): 浏览器访问端口

连接：

* 通过 VNC Viewer ：连接地址：ip:5901，默认密码: vncpassword
* 通过浏览器-访问全功能客户端：连接地址: http://localhost:6901/vnc.html, 默认密码: vncpassword
* 通过浏览器-访问轻量客户端：连接地址：http://localhost:6901/?password=vncpassword
