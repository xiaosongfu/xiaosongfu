---
title: 为 SpringBoot 应用加持 Sentry
date: 2017-01-07 19:26:01
tags: ["springboot","sentry"]
categories: ["springboot"]
---

## 目录
>  
1、配置方法  
2、参考文档  


### 1、配置方法  

>  
[sentry 官网](https://sentry.io/)，我们可以直接使用官方的服务，也可以自己搭建服务器，对于如何搭建服务器，官方有详细的文档。  

#### 1. 在 pom.xml 文件中添加依赖  

```  
<dependency>
    <groupId>com.getsentry.raven</groupId>
    <artifactId>raven-logback</artifactId>
    <version>7.8.1</version>
</dependency>
```  

#### 2. 配置 logback-spring.xml 文件  
SpringBoot 默认使用 logback 库打印日志，为了实现将日志信息发送到 sentry 服务器，我们需要配置 sentry 的 appender，所以我们需要定义 logback 的配置文件。在 src/main/resource 下新建 logback-spring.xml 文件，并按如下内容进行配置：  

```  
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <include resource="org/springframework/boot/logging/logback/base.xml"/>

    <appender name="SENTRY" class="com.getsentry.raven.logback.SentryAppender">
        <dsn>https://c82bb5298ea241a5afd458b083e6acc1:40afd81d85e04135b75a22460be5bfd7@sentry.io/127340</dsn>
        <tags>tag1:value1,tag2:value2</tags>
        <filter class="ch.qos.logback.classic.filter.ThresholdFilter">
            <level>ERROR</level>
        </filter>
    </appender>

    <root level="INFO">
        <appender-ref ref="CONSOLE"/>
        <appender-ref ref="FILE"/>
        <appender-ref ref="SENTRY"/>
    </root>
</configuration>
```  
解释：新增了一个名字为 SENTRY 的 appender，实现将 level 为 ERROR 的日志才发送到 sentry 服务器。

### 2、参考文档  
1. [官方 Logback 配置方法](https://docs.sentry.io/clients/java/modules/logback/)  
2. [Integrate Sentry in Spring-Boot application](http://www.jjoe64.com/2016/03/integrate-sentry-in-spring-boot.html)  
3. [高效利用Sentry追踪日志发现问题](https://segmentfault.com/a/1190000007890855)  
