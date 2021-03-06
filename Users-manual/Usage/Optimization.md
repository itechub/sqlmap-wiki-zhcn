# 优化

> *译自：[Optimization](https://github.com/sqlmapproject/sqlmap/wiki/Usage#optimization)*

下面的开关可以用于优化 sqlmap 的性能表现。

## 批量优化

开关：`-o`

设置这个开关表示隐含开启下面对应的选项和开关：

* `--keep-alive`
* `--null-connection`
* `--threads=3` 默认值，可以设置更大值。

查看下面内容获取更多关于开关设置的详情。

## 输出预测

开关：`--predict-output`

这个开关用于推导算法，可对获取的数据特性进行线性数据分析预测。根据 `txt/common-outputs.txt` 里面的条目及集合论相关知识预测并给出可能性最高的字符数理统计表。如果目标字符值可以在最常见的输出结果中找到，那么接下来的字符数理统计表范围会逐渐缩小。配合从 DBMS（Database Management System，数据库管理系统）中获取的实例、表名和对应的权限，那么加速效果会显著提高。当然，你可以根据自身需求对常见的输出文件进行编辑，例如，你发现了数据库表名的常见模式或者其他模式。

值得注意的是，这个开关不能够和 `--threads` 一起使用。

## HTTP Keep-Alive

开关：`--keep-alive`

这个开关参数设置 sqlmap 使用 HTTP(s) 持久化连接。

值得注意的是，这个开关不能够和 `--proxy` 一起使用。

## HTTP NULL 连接

开关：`--null-connection`

在 HTTP 请求中，存在可以获取 HTTP 响应大小而无须获取整个 HTTP 实体的特殊类型。这个技术可用于 SQL 盲注中，以区分响应结果是 `True` 还是 `False`。如果开启了这个开关，sqlmap 会测试并利用两种不同的 _NULL 连接_技术：`Range` 和 `HEAD`。如果目标服务器能够满足其中之一的请求方式，那将能够减小使用的带宽，加速整个测试过程。

这些技术的相关详情可见白皮书[提升 SQL 盲注的性能——Take 2（带宽）](http://www.wisec.it/sectou.php?id=472f952d79293)。

值得注意的是，这个开关不能和 `--text-only` 一起使用。

## 并发 HTTP(S) 请求

选项：`--threads`

sqlmap 中支持设定 HTTP(S) 请求最大并发数。
这个特性依赖于[多线程](http://en.wikipedia.org/wiki/Multithreading)，因而继承了多线程的优点和缺陷。

当数据是通过 SQL 盲注技术，或者使用暴力破解相关开关获取时，可以运用这个特性。对于 SQL 盲注技术，sqlmap 首先在单线程中计算出查询目标的长度，然后启用多线程特性，为每一个线程分配查询的一个字符。当该字符被成功获取后，线程会结束并退出——结合 sqlmap 中实现的折半算法，每个线程最多发起 7 次 HTTP(S) 请求。

考虑运行性能和目标站点的可靠性因素，sqlmap 最大的并发请求数只能设置到 **10**。

值得注意的是，这个选项不能跟 `--predict-output` 一起使用。
