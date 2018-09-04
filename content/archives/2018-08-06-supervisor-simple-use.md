---
Title: "supervisor 简单使用"
Date: 2018-08-06T10:44:45+08:00
tags: ["supervisor"]
categories: ["supervisor"]
---

## 目录
>  
1、安装  
2、创建配置文件  


Supervisor 是用在类 UNIX 操作系统上控制进程的 C/S 系统。它具有很多的功能特点：

* 简单
  Supervisor 使用 INI 样式（a simple INI-style config file）的配置文件，简单易学。它提供了很多的选型让你可以很容易的重启失败的进程，并有自动切割日志的功能。

* 集中化
  Supervisor


Supervisor 可用于 Ubuntu，Solaris，FreebSD,macOS等，也适用于类 UNIX 操作系统。但它不支持 Windows 系统。
Supervisor 使用 Python 2.4，使用 Python 3 可能不能很好的工作。


## 安装

##### 使用 `easy_install` 安装

```
easy_install supervisor
```


##### 使用 `pip` 安装

```
pip install supervisor
```

## 创建配置文件

supervisor 安装完成后，运行 `echo_supervisord_conf` 命令，可以在终端打印出配置文件示例，之后使用 `echo_supervisord_conf > /etc/supervisor/supervisord.conf` 命令创建配置文件。(需要提前创建 `/etc/supervisor/` 目录)

结果如下：

```
[root@JD link]# ls /etc/supervisor/
supervisord.conf
```

## 配置自己的配置文件

> 配置文件中的 ; 表示注释

##### 配置 [inet_http_server]

##### 配置 [inet_http_server]

配置 [inet_http_server] 后即可使用浏览器来管理。

```
[inet_http_server]          ; inet (TCP) server disabled by default
port=*:9001                 ; ip_address:port specifier, *:port for all iface
;username=user              ; default is no username (open server)
;password=123               ; default is no password (open server)
```

##### 配置 [include]


[include]
files = /etc/supervisor/conf.d/*.conf


##### 配置进程

```
mkdir -p /etc/supervisor/conf.d
```

```
vi 
```

```
;[program:theprogramname]
;command=/bin/cat              ; the program (relative uses PATH, can take args)
;process_name=%(program_name)s ; process_name expr (default %(program_name)s)
;numprocs=1                    ; number of processes copies to start (def 1)
;directory=/tmp                ; directory to cwd to before exec (def no cwd)
;umask=022                     ; umask for process (default None)
;priority=999                  ; the relative start priority (default 999)
;autostart=true                ; start at supervisord start (default: true)
;startsecs=1                   ; # of secs prog must stay up to be running (def. 1)
;startretries=3                ; max # of serial start failures when starting (default 3)
;autorestart=unexpected        ; when to restart if exited after running (def: unexpected)
;exitcodes=0,2                 ; 'expected' exit codes used with autorestart (default 0,2)
;stopsignal=QUIT               ; signal used to kill process (default TERM)
;stopwaitsecs=10               ; max num secs to wait b4 SIGKILL (default 10)
;stopasgroup=false             ; send stop signal to the UNIX process group (default false)
;killasgroup=false             ; SIGKILL the UNIX process group (def false)
;user=chrism                   ; setuid to this UNIX account to run the program
;redirect_stderr=true          ; redirect proc stderr to stdout (default false)
;stdout_logfile=/a/path        ; stdout log path, NONE for none; default AUTO
;stdout_logfile_maxbytes=1MB   ; max # logfile bytes b4 rotation (default 50MB)
;stdout_logfile_backups=10     ; # of stdout logfile backups (0 means none, default 10)
;stdout_capture_maxbytes=1MB   ; number of bytes in 'capturemode' (default 0)
;stdout_events_enabled=false   ; emit events on stdout writes (default false)
;stderr_logfile=/a/path        ; stderr log path, NONE for none; default AUTO
;stderr_logfile_maxbytes=1MB   ; max # logfile bytes b4 rotation (default 50MB)
;stderr_logfile_backups=10     ; # of stderr logfile backups (0 means none, default 10)
;stderr_capture_maxbytes=1MB   ; number of bytes in 'capturemode' (default 0)
;stderr_events_enabled=false   ; emit events on stderr writes (default false)
;environment=A="1",B="2"       ; process environment additions (def no adds)
;serverurl=AUTO                ; override serverurl computation (childutils)
```

## 常用命令

##### supervisord

启动 supervisor

```
supervisord -c /etc/supervisor/supervisord.conf
```

##### supervisorctl

```
supervisorctl reload :修改完配置文件后重新启动supervisor

supervisorctl status :查看supervisor监管的进程状态
supervisorctl status 进程名:查看supervisor监管的进程状态

supervisorctl start 进程名 ：启动XXX进程
supervisorctl start all ：

supervisorctl restart 进程名 ：重新启动XXX进程
supervisorctl restart all ：

supervisorctl stop 进程名 ：停止XXX进程
supervisorctl stop all：停止全部进程，注：start、restart、stop都不会载入最新的配置文件。

supervisorctl update：根据最新的配置文件，启动新配置或有改动的进程，配置没有改动的进程不会受影响而重启

supervisorctl tail 进程名 stdout：查看日志
```


![](https://lollipop.xiaosongfu.com/blog/201808/supervisor-action.png)  


## 配置 supervisor 开机自启动