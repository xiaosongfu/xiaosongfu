ELK 是三个开源软件的缩写，分别为：Elasticsearch 、 Logstash 以及 Kibana , 它们都是开源软件。不过现在还新增了一个 Beats，它是一个轻量级的日志收集处理工具(Agent)，Beats 占用资源少，适合于在各个服务器上搜集日志后传输给 Logstash，官方也推荐此工具，目前由于原本的 ELK Stack 成员中加入了 Beats 工具所以已改名为 Elastic Stack。


## 1. 安装

### 1.1 使用 docker 安装

参考：https://www.elastic.co/guide/en/elasticsearch/reference/current/docker.html

```
docker pull docker.elastic.co/elasticsearch/elasticsearch:6.5.4
```

### 1.2 使用压缩包安装

参考：
* https://www.elastic.co/downloads/elasticsearch
* https://www.elastic.co/guide/en/elasticsearch/reference/current/zip-targz.html

> Elasticsearch 需要 Java 8 或更高版本。可以使用 [Oracle 官方发行版](http://www.oracle.com/technetwork/java/javase/downloads/index.html)或开源 [OpenJDK](http://openjdk.java.net/)。

这里以安装 [OpenJDK](http://openjdk.java.net/) 为例，在 [OpenJDK](http://openjdk.java.net/) 官网找到下载连接，下载、解压并设置环境变量即可：

```
$ wget https://download.java.net/java/GA/jdk11/9/GPL/openjdk-11.0.2_linux-x64_bin.tar.gz

$ tar xvf openjdk-11.0.2_linux-x64_bin.tar.gz

$ vi /etc/profile
# -********-
# export JAVA_HOME=/usr/local/src/jdk-11.0.2
# export PATH=$PATH:$JAVA_HOME/bin
# -********-

$ source /etc/profile
```

下载、解压并执行：

```
$ wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.5.4.tar.gz

$ tar -xzf elasticsearch-6.5.4.tar.gz

# 这会在前台运行，需要使用 ctrl+c 退出
$ ./bin/elasticsearch

# 使用 -d 以 daemon 守护进程的方法运行
# 使用 -p pid 将进程号保存到 pid 文件
$ ./bin/elasticsearch -d -p pid

# 可以在 `$ES_HOME/logs/` 目录下查看日志
# 使用 kill `cat pid` 命令停止 elasticsearch
```

打开浏览器访问 `http://localhost:9200/`
