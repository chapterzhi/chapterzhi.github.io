---
title: go-logging
date: 2020-02-17 22:39:24
categories:
- go
tags: 
- go
---

github.com/op/go-logging  第三方日志包
使用实例如下:
```go
package main

import (
    "os"

    "github.com/op/go-logging"
)

var log = logging.MustGetLogger("example")

// Example format string. Everything except the message has a custom color
// which is dependent on the log level. Many fields have a custom output
// formatting too, eg. the time returns the hour down to the milli second.
var format = logging.MustStringFormatter(
    `%{color}%{time:15:04:05.000} %{shortfunc} > %{level:.4s} %{id:03x}%{color:reset} %{message}`,
)

// Password is just an example type implementing the Redactor interface. Any
// time this is logged, the Redacted() function will be called.
type Password string

func (p Password) Redacted() interface{} {
    return logging.Redact(string(p))
}

func main() {
    // For demo purposes, create two backend for os.Stderr.
    backend1 := logging.NewLogBackend(os.Stderr, "", 0)
    backend2 := logging.NewLogBackend(os.Stderr, "", 0)

    // For messages written to backend2 we want to add some additional
    // information to the output, including the used log level and the name of
    // the function.
    backend2Formatter := logging.NewBackendFormatter(backend2, format)

    // Only errors and more severe messages should be sent to backend1
    backend1Leveled := logging.AddModuleLevel(backend1)
    backend1Leveled.SetLevel(logging.ERROR, "")

    // Set the backends to be used.
    logging.SetBackend(backend1Leveled, backend2Formatter)

    log.Debugf("debug %s", Password("secret"))
    log.Info("info")
    log.Notice("notice")
    log.Warning("warning")
    log.Error("err")
    log.Critical("crit")
}
```

输出:
```
22:42:39.831 main > DEBU 001 debug ******
22:42:39.831 main > INFO 002 info
22:42:39.831 main > NOTI 003 notice
22:42:39.831 main > WARN 004 warning
err
22:42:39.831 main > ERRO 005 err
crit
22:42:39.831 main > CRIT 006 crit
```
![RUNOOB 图标](.\2.png "RUNOOB")