# HTTP Archive Viewer

网页版的 HTTP 请求日志分析工具。

目前主要实现了以下几点：

1. 支持以拖放（从 `Chrome` 中导出的） `.har` 文件的方式，获取原始的请求数据

2. 对请求数据中的各个阶段时间进行处理，整合出五个关键时间节点（`Queued`、`Started`、`Connection start`、`Request start` 和 `Request end`）

3. 支持按上述时间节点对请求数据进行升/降序排列

4. 展示各个请求的 HTTP 版本、 TCP 连接和优先级，并可使用 HTTP 版本进行条件筛选

## 关键时间节点

* __Queued__，请求入队列的时间。请求被加入队列之后，可能会遇到以下情况：

  * 浏览器正在分配磁盘缓存空间，它不得不等待

  * 空间分配过程中，浏览器接收到了更高优先级的请求，它被迫让道

  * 空间分配之后，因为前面的请求已经占满了可用的 TCP 连接数，它无奈等待

  * 请求在队列中等待的时间是 `Queueing`

* __Started__，请求出队列的时间。

  * 根据官方的说法，请求出队列之后，还是可能会遇到在排队阶段所列的任一种情况，从而停滞

  * 请求停滞的时间是 `Stalled`

* __Connection start__，请求和服务端连接开始的时间。

  * 停滞结束，请求进入快车道，浏览器开始 DNS 查询（`DNS Lookup`）、建立 TCP 连接（`Initial connection`）和 SSL 认证（`SSL`）等步骤

* __Request start__，请求开始的时间。

  * 请求开始之后，浏览器会发送请求（`Request sent`）、等待响应（`Waiting (TTFB)`）和接收响应（`Content Download`）。

* __Request end__，请求结束的时间。
