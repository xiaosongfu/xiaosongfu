---
title: 质量检测平台 SonarQube：1 使用 Docker 搭建服务器
date: 2017-12-07 17:15:35
tags: ["sonarqube","docker"]
categories: ["sonarqube"]
---

## 目录
>  
1、使用 Docker 搭建 SonarQube 服务  


### 1、使用 Docker 搭建 SonarQube 服务  
> SonarQube官网： [SonarQube](https://www.sonarqube.org/)  

> 官网介绍：The leading product for CONTINUOUS CODE QUALITY  

质量检测平台 SonarQube 用于管理源代码的质量，可以从七个维度检测代码质量，通过插件形式，可以支持包括java,C#,C/C++,PL/SQL,Cobol,JavaScrip,Groovy等等二十几种编程语言的代码质量管理与检测。  

SonarQube 由四大组件组成，包括一个 SonarQube 服务器，一个保存 SonarQube 服务器实例配置和所以项目代码质量报告的 SonarQube 数据库，还有 SonarQube服务器安装的各种插件，以及一个或多个运行在构建系统或持续集成系统上的用于分析项目的 SonarQube Scanners 。官网描述如下：

> 
### Architecture  
The SonarQube Platform is made of 4 components:
1. One SonarQube Server starting 3 main processes:  
    * a Web Server for developers, managers to browse quality snapshots and configure the SonarQube instance
    * a Search Server based on Elasticsearch to back searches from the UI
    * a Compute Engine Server in charge of processing code analysis reports and saving them in the SonarQube Database
2. One SonarQube Database to store:
    * the configuration of the SonarQube instance (security, plugins settings, etc.)
    * the quality snapshots of projects, views, etc.
3. Multiple SonarQube Plugins installed on the server, possibly including language, SCM, integration, authentication, and governance plugins
4. One or more SonarQube Scanners running on your Build / Continuous Integration Servers to analyze projects

架构图如下： 

![php_download_2.png](https://lollipop.xiaosongfu.com/blog/201712/sonarArch.png)  

SonarQube 服务器的安装十分简单，可以按照官方文档的步骤，先下载项目 zip 包，然后安装数据库，并修改好数据库连接信息，然后执行各个操作系统对应的启动脚本即可。坦率的讲（老罗？），这种方式我是拒绝的，所以回到主题：用 Docker 安装！

> 1 安装 postgres 数据库  
使用 Docker 安装 postgres 数据库十分的简单，只需要通过 -e 参数指定数据库的用户名和密码即可，命令如下：

```  
docker run --name sonarqubedb -e POSTGRES_USER=sonar -e POSTGRES_PASSWORD=sonar -d postgres
```  

该命令运行了一个名称为 sonarqubedb 的 postgres 容器。注意容器的名称为 sonarqubedb，因为 sonarqube 服务器连接该数据库容器时需要用到这个名字。  

> 2 安装 sonarqube 服务器  
运行如下命令：  

```  
docker run --name sonarqube --link sonarqubedb -e SONARQUBE_JDBC_URL=jdbc:postgresql://sonarqubedb:5432/sonar -p 80:9000 -d sonarqube
```  

该命令使用 -e 指定了数据库连接字符串，注意其中的数据库容器的名称；通过 -p 参数，将 sonarqube 使用的 9000 端口映射到了宿主机的80端口。  
至此，sonarqube 服务器的安装就完成了，打开浏览器该怎么访问就怎么访问吧（what？你不会？ 呃。。。）