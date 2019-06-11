---
title: "logrus 日志打印库 (2) 使用 HOOK"
date: 2018-06-12T14:41:20+08:00
tags: ["golang","log","logrus"]
categories: ["golang","log"]
---

## 目录
>  
1、使用 HOOK 输出到日志文件  
2、使用 HOOK 输出到日志文件并实现切割功能  
3、使用 HOOK 输出到 ElasticSearch  
4、使用 HOOK 实现通知 beachat 机器人


### 1、使用 HOOK 输出到日志文件

输出到日志文件需要借助 lfshook 库。使用方法很简单，可以配置日志格式，可以为不同级别的日志指定不同的日志文件。godoc 截图如下：
![你看不到我^_^](https://lollipop.xiaosongfu.com/blog/201806/lfshook.png)

指定日志文件可以通过传递文件路径或者 writer。示例：

> 方式一：指定日志文件路径

```
const logFile1 = "log/logrus/filelog1.log"

logger1 := logrus.New()

lfsHook := lfshook.NewHook(lfshook.PathMap{
    logrus.DebugLevel: logFile1,
    logrus.InfoLevel:  logFile1,
    logrus.WarnLevel:  logFile1,
    logrus.ErrorLevel: logFile1,
    logrus.FatalLevel: logFile1,
    logrus.PanicLevel: logFile1,
}, &logrus.TextFormatter{})
logger1.AddHook(lfsHook)

logger1.Debug("file log text-debug")
logger1.Info("file log text-info")
```

> 方式二：指定 file writer

```
const logFile2 = "log/logrus/filelog2.log"
logger2 := logrus.New()

var fileWriter io.Writer
fileWriter, err := os.OpenFile(logFile2, os.O_CREATE|os.O_WRONLY, 0666)
if err != nil {
    fileWriter = os.Stderr
}
lfsHook := lfshook.NewHook(lfshook.WriterMap{
    logrus.DebugLevel: fileWriter,
    logrus.InfoLevel:  fileWriter,
    logrus.WarnLevel:  fileWriter,
    logrus.ErrorLevel: fileWriter,
    logrus.FatalLevel: fileWriter,
    logrus.PanicLevel: fileWriter,
}, &logrus.TextFormatter{})
logger2.AddHook(lfsHook)

logger2.Debug("file log text-debug")
logger2.Info("file log text-info")
```

PathMap 和 WriterMap 的定义如下：

```
// PathMap is map for mapping a log level to a file's path.
// Multiple levels may share a file, but multiple files may not be used for one level.
type PathMap map[logrus.Level]string

// WriterMap is map for mapping a log level to an io.Writer.
// Multiple levels may share a writer, but multiple writers may not be used for one level.
type WriterMap map[logrus.Level]io.Writer
```

就是这么简单。

### 2、使用 HOOK 输出到日志文件并实现切割功能

实现输出到日志文件并切割只需要在 lfshook 库基础上将 WriterMap 中传入的 writer 不明确在某一个文件上即可，可以借助 github.com/lestrrat-go/file-rotatelogs 库来实现。直接上代码:

```
package main

import (
	"time"

	"github.com/lestrrat-go/file-rotatelogs"
	"github.com/rifflock/lfshook"
	"github.com/sirupsen/logrus"
)

func main() {
	var logFilePath = "log/logrux"
	var logFileName = "access"

	logger := logrus.New()

	rotateLogs, err := rotatelogs.New(logFilePath+logFileName+".%Y%m%d%H%M",
		rotatelogs.WithLinkName(logFileName),
		rotatelogs.WithMaxAge(24*time.Hour),
		rotatelogs.WithRotationTime(time.Hour),
	)
	if err != nil {
		panic(err)
	}

	lfsHook := lfshook.NewHook(lfshook.WriterMap{
		logrus.DebugLevel: rotateLogs,
		logrus.InfoLevel:  rotateLogs,
		logrus.WarnLevel:  rotateLogs,
		logrus.ErrorLevel: rotateLogs,
		logrus.FatalLevel: rotateLogs,
		logrus.PanicLevel: rotateLogs,
	}, &logrus.TextFormatter{})
	
	logger.AddHook(lfsHook)

	logger.Debug("file log text-debug")
	logger.Info("file log text-info")
}
```

