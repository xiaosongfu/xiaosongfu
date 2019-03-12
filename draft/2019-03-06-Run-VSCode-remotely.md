---

* https://github.com/codercom/code-server
* https://coder.com/
* http://ide.coder.com/

---

* https://www.theia-ide.org/
* https://github.com/theia-ide/theia

---

在远程服务器运行 VS code，并通过浏览器访问，这可以：

1. 在 Linux 等服务器上运行后，可以通过电脑、平板等设备随时随地写代码
2. 如果部署在香港等地方，则可以直接服务器墙外面的资源


## 1. 安装

v1.31.0

```
$ wget https://github.com/codercom/code-server/releases/download/1.31.0/code-server-1.31.0-x86_64-linux.tar.gz
```

```
$ tar zxvf code-server-1.31.0-x86_64-linux.tar.gz
code-server-1.31.0-x86_64-linux/
code-server-1.31.0-x86_64-linux/README.md
code-server-1.31.0-x86_64-linux/code-server
code-server-1.31.0-x86_64-linux/LICENSE

$ tree
.
|-- code-server-1.31.0-x86_64-linux
|   |-- code-server
|   |-- LICENSE
|   `-- README.md
`-- code-server-1.31.0-x86_64-linux.tar.gz

1 directory, 4 files
```

```
$ ./code-server --help
Start your own self-hosted browser-accessible VS Code

USAGE
  $ server [WORKDIR]

ARGUMENTS
  WORKDIR  [default: /Users/fuxiaosong/Downloads/code-server-1.31.0-x86_64-apple-darwin] Specify working dir

OPTIONS
  -d, --data-dir=data-dir
  -h, --host=host          [default: 0.0.0.0]
  -o, --open               Open in browser on startup
  -p, --port=port          [default: 8443] Port to bind on
  -v, --version            show CLI version
  --allow-http
  --cert=cert
  --cert-key=cert-key
  --help                   show CLI help
  --no-auth
```