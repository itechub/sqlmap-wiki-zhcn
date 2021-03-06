# 技术

> *译自：[Techniques](https://github.com/sqlmapproject/sqlmap/wiki/Usage#techniques)*

以下选项可用于调整对特定 SQL 注入技术的测试。

## 测试会用到的 SQL 注入技术

选项：`--technique`

此选项用于指定需要测试的 SQL 注入类型。默认情况下 sqlmap 会测试它支持的**所有**类型/技术。

在某些情况下，你可能只想测试一种或几种特定类型的 SQL 注入，这便是该选项存在的作用。

此选项需要一个参数值。该参数是由 `B`，`E`，`U`，`S`，`T` 和 `Q` 这样的字符任意组合成的字符串，每个字母代表不同的技术：

* `B`：布尔型盲注（Boolean-based blind）
* `E`：报错型注入（Error-based）
* `U`：联合查询注入（UNION query-based）
* `S`：堆叠查询注入（Stacked queries）
* `T`：时间型盲注（Time-based blind）
* `Q`：内联查询注入（inline Query）

例如，如果仅测试利用报错型注入和堆叠查询注入，你可以提供 `ES`。默认值为 `BEUSTQ`。

注意，当你需要访问文件系统，接管操作系统或访问 Windows 注册表配置单元时，提供的字符串必须包含代表堆叠查询技术的字母 `S`。

## 设置时间型盲注中 DBMS（Database Management System，数据库管理系统）延迟响应秒数

选项：`--time-sec`

为 `--time-sec` 提供一个整数，可以设置时间型盲注响应的延迟时间。默认情况下，它的值为 **5 秒**。

## 指定联合查询注入中的列数

选项：`--union-cols`

默认情况下，sqlmap 进行联合查询注入时使用 1 到 10 列。当然，可以通过提供更高的`--level` 值将该范围增加到最多 50 列。有关详细信息，请参阅相关段落。

你可以手动指定选项 `--union-cols` 和相应的数字范围，以针对该类型的 SQL 注入测试特定范围的列。例如，`12-16` 代表进行 12 到 16 列的联合查询注入测试。

## 用于测试联合查询注入的字符

选项：`--union-char`

默认情况下，sqlmap 测试联合查询注入会使用 `NULL` 字符。然而，通过提供更高的`--level` 值，sqlmap 将执行一个随机数字的测试，因为存在少数情况，使用 `NULL` 的联合查询注入会失败，而使用随机整数会成功。

你可以手动提供选项 `--union-char` 和所需的数字（例如：`--union-char 123`）来测试该类型的 SQL 注入。

## 联合查询注入中 FROM 子句中使用的表

选项：`--union-from`

在部分联合查询注入中，需要在 `FROM` 子句中强制指定使用有效且可访问的表名。例如，Microsoft Access 就要求使用这样的表。如果不提供一个这样的表，联合查询注入将无法正常执行（例如：`--union-from=users`）。

## DNS 渗出攻击

选项：`--dns-domain`

DNS 渗出 SQL 注入攻击在文章 [Data Retrieval over DNS in SQL Injection Attacks](http://arxiv.org/pdf/1303.3047.pdf)（译者注：乌云知识库有该文章的翻译，[在 SQL 注入中使用 DNS 获取数据](http://cb.drops.wiki/drops/tips-5283.html)）中进行了介绍，而 sqlmap 中的实现方式可以在幻灯片 [使用 sqlmap 进行 DNS 渗出攻击](http://www.slideshare.net/stamparm/dns-exfiltration-using-sqlmap-13163281) 中找到。

如果用户正控制着一台注册为 DNS 域名服务器的主机（例如：域名 `attacker.com`），则可以使用该选项（例如：`--dns-domain attacker.com`）来启用此攻击。它的前提条件是使用 `Administrator`（即管理员）权限（因为需要使用特权端口 `53`）运行 sqlmap，这时可以使用常用的（盲注）技术来进行攻击。如果已经识别出一种有效攻击方式（最好是时间型盲注），则这种攻击能够加速获取数据的过程。如果报错型注入或联合查询注入技术可用，则默认情况下将跳过 DNS 渗出攻击测试。

## 二阶注入攻击

选项：`--second-url` 与 `--second-req`

当攻击一个存在漏洞的页面，它的 payload 注入结果显示（反射）在另一个页面（例如：frame）中，这种攻击就叫 SQL 二阶注入攻击。通常情况是用户在存在漏洞的页面输入内容存储到数据库而导致的漏洞。

你可以使用选项 `--second-url` 加上结果显示页面的 URL 地址，或者使用 `--second-req` 加上相应的请求文件路径，以此来测试此类型的 SQL 注入。
