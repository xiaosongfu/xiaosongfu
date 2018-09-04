---
title: "logrus 日志打印库 (1) 简单使用"
date: 2018-06-12T14:33:40+08:00
tags: ["golang","log","logrus"]
categories: ["golang","log"]
---

## 目录
>  
1、简介  
2、简单使用  
3、基本使用  
4、API 概览  
5、参考文章  


### 1、简介
logrus 是结构化的，与 golang 标准库完全兼容的 golang 日志打印框架。支持 Filed 和 HOOK 机制，自带 text 和 json 两种日志输出格式。

### 2、简单使用
最简单的方法就是直接使用包级暴露的打印方法。直接上代码：
```
package main

import "github.com/sirupsen/logrus"

func main() {
    logrus.WithFields(logrus.Fields{
        "animal": "walrus",
        "number": 1,
        "size":   10,
    }).Info("A walrus appears")
}
```
输出：
```
time="2018-06-08T15:32:35+08:00" level=info msg="A walrus appears" animal=walrus number=1 size=10
```

也可以将日志打印到文件：

```
log := logrus.New()
file, err := os.OpenFile("logrus.log", os.O_CREATE|os.O_WRONLY, 0666)
if err == nil {
    log.Out = file
} else {
    log.Info("Failed to log to file, using default stderr")
}
log.Info("log-- log--")
```

### 3、基本使用
##### 3.1 设置日志输出格式
logrus 自带两种格式 logrus.JSONFormatter{} 和 logrus.TextFormatter{}，设置方法如下：
```
logrus.SetFormatter(&logrus.JSONFormatter{})
logrus.Infoln("JSONFormatter")

logrus.SetFormatter(&logrus.TextFormatter{})
logrus.Infoln("TextFormatter not time")
```

可以自己创建一个 logrus.Logger，并设置它的日志格式：
```
log := logrus.New()
log.Formatter= new(logrus.JSONFormatter)
log.Infoln("my logger")
```
##### 3.2 设置日志打印级别
logrus 提供 6 档日志级别，分别是：
```
PanicLevel
FatalLevel
ErrorLevel
WarnLevel
InfoLevel
DebugLevel
```

附一条阮一峰老师关于日志级别的博客：  
@ruanyf  

```
日志的级别（来自@dylanbeattie）
- Fatal：网站挂了，或者极度不正常
- Error：跟遇到的用户说对不起，可能有bug
- Warn：记录一下，某事又发生了
- Info：提示一切正常
- debug：没问题，就看看堆栈
```

使用 SetLevel() 方法设置：
```
logrus.SetLevel(logrus.DebugLevel)
logrus.Debug("DebugLevel")// 会打印出来
logrus.SetLevel(logrus.ErrorLevel)
logrus.Debug("ErrorLevel") // 不会打印出来
```

可以自己创建一个 logrus.Logger，并设置它的日志级别：
```
log := logrus.New()
log.Level = logrus.DebugLevel
log.Debug("DebugLevel")// 会打印出来
log.Level = logrus.WarnLevel
log.Debug("WarnLevel")// 不会打印出来
```

##### 3.3 自定义 Field
logrus 默认的日志输出有 time、level 和 msg 3个 Field，其中 time 可以不显示，方法如下：
```
logrus.SetFormatter(&logrus.TextFormatter{DisableTimestamp: true})
```

自定义 Field 的方法如下：
```
// 只用一次
logrus.WithFields(logrus.Fields{"key1": "11", "key2":22}).Infoln("info...")

// 可以多次使用
entry := logrus.WithFields(logrus.Fields{"key": "value"})
entry.Infoln("info...")
```

##### 3.4 自定义 Logger
golang 有一条约定俗成的规范，就是可以让使用者实例化的 struce 都会提供一个 New 方法，返回一个该 struce 的指针，logrus.Logger 结构体也一样，Logger 结构体的定义如下：

```
type Logger struct {
	Out io.Writer
	Hooks LevelHooks
	Formatter Formatter
	Level Level
	mu MutexWrap
	entryPool sync.Pool
}
```

New() 方法的定义如下：
```
func New() *Logger {
	return &Logger{
		Out:       os.Stderr,
		Formatter: new(TextFormatter),
		Hooks:     make(LevelHooks),
		Level:     InfoLevel,
	}
}
```

LevelHooks 类型的定义如下：
```
// Internal type for storing the hooks on a logger instance.
type LevelHooks map[Level][]Hook
```

New 一个 Logger 来使用很简单：
```
log := logrus.New()
log.Debug("debug...")
```

### 4、API 概览
##### 4.1 包级暴露的各日志级别打印方法

```
func Print(args ...interface{})
func Printf(format string, args ...interface{})
func Println(args ...interface{})

func Debug(args ...interface{})
func Debugf(format string, args ...interface{})
func Debugln(args ...interface{})

func Info(args ...interface{})
func Infof(format string, args ...interface{})
func Infoln(args ...interface{})

func Warn(args ...interface{})
func Warnf(format string, args ...interface{})
func Warnln(args ...interface{})

func Warning(args ...interface{})
func Warningf(format string, args ...interface{})
func Warningln(args ...interface{})

func Error(args ...interface{})
func Errorf(format string, args ...interface{})
func Errorln(args ...interface{})

func Fatal(args ...interface{})
func Fatalf(format string, args ...interface{})
func Fatalln(args ...interface{})

func Panic(args ...interface{})
func Panicf(format string, args ...interface{})
func Panicln(args ...interface{})

func Exit(code int)
```
##### 4.2 其他包级暴露的方法

```
func AddHook(hook Hook)

func RegisterExitHandler(handler func())

func SetFormatter(formatter Formatter)

func SetLevel(level Level)

func SetOutput(out io.Writer)
```

### 5、参考文章
[golang 日志包logrus详解](http://baijiahao.baidu.com/s?id=1597892026895003802)  
[Logrus的使用](https://www.jianshu.com/p/5fac8bed4505)  
[golang日志框架logrus](https://studygolang.com/articles/10534)  

[logrus](https://github.com/sirupsen/logrus)  
[logrus doc](https://godoc.org/github.com/sirupsen/logrus)  
[lfshook](https://github.com/rifflock/lfshook)  
[lfshook doc](https://godoc.org/github.com/rifflock/lfshook)  
[file-rotatelogs](https://github.com/lestrrat-go/file-rotatelogs)  
[file-rotatelogs doc](https://godoc.org/github.com/lestrrat-go/file-rotatelogs)  