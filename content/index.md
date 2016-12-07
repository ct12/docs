---
weight: 1
---

# 简介

> StockDB 工作原理：

```
                 ticker or OHLC record
                           +
                           |
     +---------------------+---------------------+
     |                     |                     |
     |                     |                     |
     |           +---------v---------+           |
     |           |Collection Services|           |
     |           +---------+---------+           |
     |                     |                     |
     |  S                  |(store)              |
     |  T                  |                     |
     |  O     +------------v------------+        |
     |  C     |InfluxDB OR ElasticSearch|        |
     |  K     +------------+------------+        |
     |  D                  |                     |
     |  B                  |(query)              |
     |                     |                     |
     |            +--------v--------+            |
     |            |Analysis Services|            |
     |            +--------+--------+            |
     |                     |                     |
     |                     |                     |
     +---------------------+---------------------+
                           |
                           v
       multi-period OHLC record, market depth...
```

StockDB 是一个处理交易类数据的数据库解决方案，实现了K线变频、模拟历史市场深度、模拟交易等功能。

### 下载

| 操作系统 | 地址 |
| -------- | ---- |
| Linux 64bit | `http://download.stockdb.org/stockdb.latest.linux-amd64.tar.gz` |
| MacOS 64bit | `http://download.stockdb.org/stockdb.latest.darwin-amd64.tar.gz` |
| Windows 64bit | `http://download.stockdb.org/stockdb.latest.windows-amd64.zip` |
| Linux 32bit | `http://download.stockdb.org/stockdb.latest.linux-386.tar.gz` |
| Windows 32bit | `http://download.stockdb.org/stockdb.latest.windows-amd64.zip` |

<aside class="warning">
目前由于 API 还不稳定，所以上面的下载地址只是演示地址，并不能下载。
</aside>

### 安装

解压后直接运行。

# 基本概念

为方便理解 StockDB 的一些概念，下面做一些类比：

| StockDB | MySQL | InfluxDB | ELasticSearch | 解释 | 示例 |
| ------- | ----- | -------- | ------------- | ---- | ---- |
| Market | Database | Database | Index | 数据所属的市场 | `nasdaq`，`上证` |
| Symbol | Tabel | Measurement | Type | 数据的分类标识 | `aapl`，`平安银行` |

<aside class="warning">
StockDB 依赖 <code>InfluxDB</code> 或 <code>ELasticSearch</code> 存储底层数据，请确保有正常运行的 <code>InfluxDB</code> 或 <code>ELasticSearch</code> 。
</aside>

# 客户端

```go
// go get github.com/miaolz123/stockdb/stockdb

package main

import (
	"github.com/miaolz123/stockdb/stockdb"
)

func main() {
	client := stockdb.New("http://localhost:8765", "username:password")
	// balabala
}
```

```js
// npm i stockdb -S

import StockDB from 'stockdb';

const client = StockDB.New('http://localhost:8765', 'username:password');

// balabala
```

> 注意：把`"http://localhost:8765"`和`"username:password"`自己的 StockDB 服务端地址和验证。

对外提供多个编程语言的API接口，支持：

- [JavaScript（官方）](https://github.com/stockdb/stockdb-js)
- [Golang（官方）](https://github.com/miaolz123/stockdb/tree/master/stockdb)
- Python
- Java
- Ruby
- PHP
- C++
- C#
