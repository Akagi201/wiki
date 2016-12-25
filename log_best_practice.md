# 日志

## 日志格式
* 统一使用 [logfmt](https://brandur.org/logfmt) 格式.
* 时间格式使用 [RFC3339](https://datatracker.ietf.org/doc/rfc3339/). `YY-MM-DDTHH:MM:SS.nnnnnnnnnZ`

## 日志的作用
* troubleshooting
* 显示程序运行状态
* 计费.

## 日志分类
* debug 日志, 用于 troubleshooting.
* 关键点日志, 用于显示程序运行状态.
* 计费日志, 用于计费.
* 操作日志.

## 哪些地方需要记录日志.
* 对外部系统与模块的调用前后.
* 状态变化. 关键变量, 以及正在做哪些重要额事情, 通过日志了解程序此时在什么状态中.
* 模块或系统的输入与输出.
* 业务异常.
* 非预期执行, "不可能"执行到的地方, 如, 很少出现的 else 情况.
* 大批量数据的执行进度.

## 好的日志
* 使用唯一的 ID.
* 带上 context, human readable msg.
* 异常与错误, 要能够定位到源码.

## 应该避免哪些日志.
* 临时 debug 日志.
* 混淆信息的日志.
* 使用统一的日志函数.
* 正确的错误级别.
* 遗漏了关键信息, 如: go 的 error 应增加 context.
* 过多的日志会影响程序定位问题, 以及程序的性能.
* 不要日志 + 循环. 避免写入无限量的日志数据.

## Refs
* [splunk Logging best practices](http://dev.splunk.com/view/logging-best-practices/SP-CAAADP6)
* [Discover Logging Best Practices Part 1: Collecting Logs](https://logmatic.io/beyond-application-monitoring-discover-logging-best-practices/)
* [Discover Logging Best Practices Part 2: Application Troubleshooting & Investigation](https://logmatic.io/discover-logging-best-practices-application-troubleshooting/)
