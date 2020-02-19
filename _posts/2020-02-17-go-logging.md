---
title: go-logging
date: 2020-02-17 22:39:24
categories:
- go
tags: 
- go-logging
---

github.com/op/go-logging  第三方日志包  
使用实例如下:
```go
package main

import (
    "os"

    "github.com/op/go-logging"               // go-logging的包
)

var log = logging.MustGetLogger("example")   // 通过MustGetLogger()返回一个log对象，参数为对logger的命名

// Example format string. Everything except the message has a custom color
// which is dependent on the log level. Many fields have a custom output
// formatting too, eg. the time returns the hour down to the milli second.
var format = logging.MustStringFormatter(
    `%{color}%{time:15:04:05.000} %{shortfunc} > %{level:.4s} %{id:03x}%{color:reset} %{message}`,
)                                            // 设置logger输出格式         22:42:39.831 main > INFO 002 info



// Password is just an example type implementing the Redactor interface. Any
// time this is logged, the Redacted() function will be called.
type Password string

func (p Password) Redacted() interface{} {
    return logging.Redact(string(p))
}

func main() {
    // For demo purposes, create two backend for os.Stderr.
    backend1 := logging.NewLogBackend(os.Stderr, "", 0)    // 设置第一个后端，输出到标准错误输出
    backend2 := logging.NewLogBackend(os.Stderr, "", 0)    // 设置第二个后端，同样输出到标准错误输出

    // For messages written to backend2 we want to add some additional
    // information to the output, including the used log level and the name of
    // the function.
    backend2Formatter := logging.NewBackendFormatter(backend2, format)    //将第二个后端和之前定义的输出格式绑定

    // Only errors and more severe messages should be sent to backend1
    backend1Leveled := logging.AddModuleLevel(backend1)
    backend1Leveled.SetLevel(logging.ERROR, "")                           //设置从第一个终端输出的错误等级，只有errors和更严重的错误才会从第一个终端输出

    // Set the backends to be used.
    logging.SetBackend(backend1Leveled, backend2Formatter) // 设置终端，这里设置了两个，可以只设置一个

    log.Debugf("debug %s", Password("secret"))             //Debug 后加 f 是为了将 %s 格式化处理
    log.Info("info")
    log.Notice("notice")
    log.Warning("warning")
    log.Error("err")
    log.Critical("crit")
}
```

输出1:
```
22:42:39.831 main > DEBU 001 debug ******
22:42:39.831 main > INFO 002 info
22:42:39.831 main > NOTI 003 notice
22:42:39.831 main > WARN 004 warning
err                                                 // A 
22:42:39.831 main > ERRO 005 err
crit                                                // B   A、B两行为第一个终端的输出，没有设置格式，错误等级为errors以上才输出，其他行为第二个终端的输出
22:42:39.831 main > CRIT 006 crit
```
![输出](https://github.com/chapterzhi/photo/blob/master/blog/1.png)