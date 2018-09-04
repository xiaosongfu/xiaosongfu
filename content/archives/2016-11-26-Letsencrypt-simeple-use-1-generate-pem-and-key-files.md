---
title: Letsencrypt的简单使用 1-证书的生成和配置
date: 2016-11-26 16:23:47
tags: ["Letsencrypt"]
categories: ["Letsencrypt"]
---

## 目录
>  
1、下载letsencrypt客户端  
2、生成证书  
3、配置nginx的https服务器  


### 1、下载letsencrypt客户端  
需要使用git命令下载letsencrypt客户端，步骤和命令如下：
```  
git clone https://github.com/letsencrypt/letsencrypt
cd letsencrypt
```  
### 2、生成证书
命令：
```  
./letsencrypt-auto
```  
该命令的执行需要很多的依赖库，但是我们并不需要关心，因为它会找到系统的yum命令的目录，然后自动帮我们安装。  
有些库yum源里面可能找不到而无法安装，我在执行的时候就出错了，提示信息如下：  
```  
Loading mirror speeds from cached hostfile
* base: centos.mirrors.hoobly.com
* extras: mirror.pac-12.org
* updates: mirrors.easynews.com
No package python-virtualenv available.
No package python-pip available.
```  
对于这个问题，可以通过安装epel解决，安装好epel之后再重新执行 ./letsencrypt-auto 即可解决。安装epel的命令是：yum install epel-release  

所有依赖库自动安装完成后，在执行的时候还是报错了，提示说是因为Python需要2.7以上版本，但我的系统是2.6.6。但是yum又需要Python是2.6.6的版本，所以我没有选择升级Python，而是换一个命令生成证书（当然是可以通过升级Python来实现证书的生成）。报错信息如下：
```  
Requesting root privileges to run letsencrypt…
/root/.local/share/letsencrypt/bin/letsencrypt
/root/.local/share/letsencrypt/lib/python2.6/site-packages/cryptography/__init__.py:26: DeprecationWarning: Python 2.6 is no longer supported by the Python core team, please upgrade your Python. A future version of cryptography will drop support for Python 2.6
DeprecationWarning
Version: 1.1-20080819
Version: 1.1-20080819
/root/.local/share/letsencrypt/lib/python2.6/site-packages/letsencrypt/main.py:450: DeprecationWarning: BaseException.message has been deprecated as of Python 2.6
return e.message
No installers are available on your OS yet; try running “letsencrypt-auto certonly” to get a cert you can install manually
```  
它告诉我们可以使用  letsencrypt-auto certonly  命令来获取证书。接着我们执行以下命令：
```  
./letsencrypt-auto certonly
```  
接下来的操作是图形化的，我把图片都附上来了：

![1](https://lollipop.xiaosongfu.com/blog/201611/letsencrypt-1.PNG)  

![2](https://lollipop.xiaosongfu.com/blog/201611/letsencrypt-2.PNG)  

![3](https://lollipop.xiaosongfu.com/blog/201611/letsencrypt-3.PNG)  

![4](https://lollipop.xiaosongfu.com/blog/201611/letsencrypt-4.PNG)  

![5](https://lollipop.xiaosongfu.com/blog/201611/letsencrypt-5.PNG)  

![6](https://lollipop.xiaosongfu.com/blog/201611/letsencrypt-6.png)  

至此，证书的生成就完成了。最后一张图片告诉了我们证书的证书的存放位置，我们切换到那个目录并查看一下，其中/etc/letsencrypt/live/www.fuxiaosong.com/fullchain.pem 和 privkey.pem就是我们需要配置到nginx的。
```  
[root@Centos6 fuxiaosong]# cd /etc/letsencrypt/
[root@Centos6 letsencrypt]# ls
accounts  archive  csr  keys  live  renewal
[root@Centos6 letsencrypt]# tree
.
|-- accounts
|   `-- acme-v01.api.letsencrypt.org
|       `-- directory
|           `-- e40d7747a591f6923e4a65a4b7199ec1
|               |-- meta.json
|               |-- private_key.json
|               `-- regr.json
|-- archive
|   `-- www.fuxiaosong.com
|       |-- cert1.pem
|       |-- chain1.pem
|       |-- fullchain1.pem
|       `-- privkey1.pem
|-- csr
|   `-- 0000_csr-letsencrypt.pem
|-- keys
|   `-- 0000_key-letsencrypt.pem
|-- live
|   `-- www.fuxiaosong.com
|       |-- cert.pem -> ../../archive/www.fuxiaosong.com/cert1.pem
|       |-- chain.pem -> ../../archive/www.fuxiaosong.com/chain1.pem
|       |-- fullchain.pem -> ../../archive/www.fuxiaosong.com/fullchain1.pem
|       `-- privkey.pem -> ../../archive/www.fuxiaosong.com/privkey1.pem
`-- renewal
    `-- www.fuxiaosong.com.conf

11 directories, 14 files
[root@Centos6 letsencrypt]#
```  

### 3、配置nginx的https服务器
nginx的配置文件里就有https服务器的配置示例，我们只需要设置好.pem文件的位置即可。  
https服务器的配置示例：
```  
# HTTPS server
    #
    #server {
    #    listen       443 ssl;
    #    server_name  localhost;

    #    ssl_certificate      cert.pem;
    #    ssl_certificate_key  cert.key;

    #    ssl_session_cache    shared:SSL:1m;
    #    ssl_session_timeout  5m;

    #    ssl_ciphers  HIGH:!aNULL:!MD5;
    #    ssl_prefer_server_ciphers  on;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}
```  
修改为以下配置即可：
```  
# HTTPS server
    #
    server {
        listen       443 ssl;
        server_name  localhost;

        ssl_certificate      /etc/letsencrypt/live/wwww.fuxiaosong.com/fullchain.pem;
        ssl_certificate_key  /etc/letsencrypt/live/wwww.fuxiaosong.com/privkey.pem;

        ssl_session_cache    shared:SSL:1m;
        ssl_session_timeout  5m;

        ssl_ciphers  HIGH:!aNULL:!MD5;
        ssl_prefer_server_ciphers  on;

        location / {
            root   html;
            index  index.html;
        }
    }
```  
使用浏览器访问https://www.fuxiaosong.com，可以看到网站的证书是受信任的。

![7](https://lollipop.xiaosongfu.com/blog/201611/letsencrypt-7.PNG)  

还遗留2个问题：
>  
1. letsencrypt的证书有效期只有3个月，到期后需要更新证书。
2. 直接输入域名www.fuxiaosong.com或fuxiaosong.com都是使用http协议而不是https协议，还需要配置一下。
