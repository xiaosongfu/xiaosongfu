---
title: Nginx-PHP 的安装
date: 2016-11-26 18:17:43
tags: ["nginx","php"]
categories: ["nginx","php"]
---

## 目录
>  
1、php的安装  
2、nginx的安装  


![nginx-php-fpm.png](https://lollipop.xiaosongfu.com/blog/201611/nginx-php-fpm.png)  

### 1、php的安装
#### 1.1 下载PHP源码包  
进入php下载页面，根据自己的喜好，下载任意一种压缩格式的源码包都可以。

![php_download_1.png](https://lollipop.xiaosongfu.com/blog/201611/php_download_1.png)

选择一个就近的站点下载源码包即可。

![php_download_2.png](https://lollipop.xiaosongfu.com/blog/201611/php_download_2.png)  

#### 1.2 安装依赖  
```  
yum install libxml2 libxml2-devel
yum install bzip2 bzip2-devel
yum install curl curl-devel
yum install libmcrypt libmcrypt-devel mcrypt mhash
yum install readline readline-devel
```  
如果不先安装这些依赖，则在执行configure命令时会报错误，所以最好的方式是先安装依赖。

#### 1.3 编译安装PHP  
①先解压php源码包：tar -zxvf php  
②切换到解压后的目录下，执行configure命令：
```  
./configure \
--prefix=/usr/local/php \
--enable-fpm
```  
如果没有错误的话，结果如下图：

![php1.png](https://lollipop.xiaosongfu.com/blog/201611/php1.png)

③没有错误就继续下一步，即是编译：
```  
make
```  
如果没有错误的话，结果如下图：

![php2.png](https://lollipop.xiaosongfu.com/blog/201611/php2.png)

④没有错误就继续下一步，即是安装：
```  
make install
```  
如果没有错误的话，结果如下图：

![php3.png](https://lollipop.xiaosongfu.com/blog/201611/php3.png)

#### 1.4 启动PHP-FPM
PHP-FPM是一个PHP FastCGI管理器，PHP-FPM提供了更好的PHP进程管理方式，可以有效控制内存和进程、可以平滑重载PHP配置，比spawn-fcgi具有更多优点，所以被PHP官 方收录了。  
新版PHP已经集成php-fpm了，不再作为第三方包。  
在./configure的时候带 –enable-fpm参数即可开启PHP-FPM。

![php-fpm.png](https://lollipop.xiaosongfu.com/blog/201611/php-fpm.png)

A、配置和启动php-fpm的方法如下：
```  
cd /usr/local/php/etc
cp php-fpm.conf.default php-fpm.conf
/usr/local/php/sbin/php-fpm
```  
编译安装完成后，php-fpm 的备份配置文件在 /usr/local/php/etc 目录下，所以先切换到该目录，然后使用 cp 命令创建其配置文件 php-fpm.conf，最后通过执行 /usr/local/php/sbin/php-fpm 命令来启动 php-fpm，启动后他默认监听9000端口。

B、nginx 将 php 文件交给 php 引擎来处理是需要配置的，就如 apache 和 php 配合的时候 apache 需要载人 php 模块类似，为了使 nginx 和 php 配合工作，需要在 nginx 的配置文件中配置，示例配置如下：
```  
server {
        listen       80;

        server_name nginx.ifuxs.cn;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
            root   html/nginx;
            index  index.php index.htm;
        }

        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

        # proxy the PHP scripts to Apache listening on 127.0.0.1:80
        #
        #location ~ .php$ {
        #    proxy_pass   http://127.0.0.1;
        #}

        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        location ~ .php$ {
            root           html/nginx;
            fastcgi_pass   127.0.0.1:9000;
            fastcgi_index  index.php;
            fastcgi_param  SCRIPT_FILENAME $document_root$fastcgi_script_name;
            include        fastcgi_params;
        }

        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /.ht {
        #    deny  all;
        #}
    }
```  
在配置的时候，它对php文件报了File not found.错误，经过排除发现是fastcgi_param的问题，nginx编译安装完成后是
```  
fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
```  
需要修改为：
```  
fastcgi_param  SCRIPT_FILENAME $document_root$fastcgi_script_name;
```  
#### 1.5 完成  
至此，PHP的源码编译安装就完成了

### 2、nginx的安装  
Nginx服务器的性能较高的原因是其采用了epoll的IO模型有关，apache和IIS都是采用的select的IO模型。其他的IO模型还有kqueue等。
#### 2.1 下载源代码

![nginx_download.png](https://lollipop.xiaosongfu.com/blog/201611/nginx_download.png)  

命令示例：
```  
wget wget http://nginx.org/download/nginx-1.8.0.tar.gz
```  
#### 2.2 解压源码
```  
tar -zxvf nginx-1.8.0.tar.gz
```  
#### 2.3 编译安装
①依赖的安装  
I：为了支持https协议，需要OpenSSL的支持，所以需要下载[OpenSSL](http://www.openssl.org/)源码，但不要求安装;  
II：nginx的rewrite功能模块和配置文件中的location都需要使用正则表达式，所以也需要[pcre](http://www.pcre.org/)的支持，只需下载pcre源码，不要求安装;  
III：http响应头的压缩需要zlib库的支持，所以还需要下载[zlib](http://zlib.net/)库，但不要求安装。  
这3个库的源码只需到官网下载，然后解压即可，在编译安装nginx的时候，带上参数：
```  
--with-pcre=/usr/local/src/pcre-8.21
--with-zlib=/usr/local/src/zlib-1.2.8
--with-openssl=/usr/local/src/openssl-1.0.1c
```  
即可，但是需要注意版本号要为自己所下载的源码包对应的版本号。

②执行configure命令
configure命令如下：
```  
./configure \
--prefix=/usr/local/nginx \
--sbin-path=/usr/local/nginx/sbin/nginx \
--conf-path=/usr/local/nginx/conf/nginx.conf \
--error-log-path=/usr/local/nginx/logs/error.log \
--http-log-path=/usr/local/nginx/logs/access.log \
--pid-path=/usr/local/nginx/var/nginx.pid \
--with-http_ssl_module \
--with-http_gzip_static_module \
--with-http_stub_status_module \
--with-pcre=/usr/local/src/pcre-8.36 \
--with-zlib=/usr/local/src/zlib-1.2.8 \
--with-openssl=/usr/local/src/openssl-1.0.1p
```  
③编译：
```  
make
```  
④安装：
```  
make install
```  

#### 2.4 NGINX服务的启动和关闭
①检查配置文件是否正确：
```  
/usr/local/nginx/sbin/nginx -t
```  
②启动：
```  
# 使用默认的配置文件启动服务
/usr/local/nginx/sbin/nginx
#指定配置文件来启动服务
/usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf
```  
③停止：
```  
/usr/local/nginx/sbin/nginx -s stop
```  
④修改配置文件后重新加载配置文件：
```  
/usr/local/nginx/sbin/nginx -s reload #重启，不改变配置文件
```  
