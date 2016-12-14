---
weight: 1
---

# 简介

[![GitHub release](https://img.shields.io/github/release/miaolz123/stockdb.svg)](https://github.com/miaolz123/stockdb/releases)
[![Travis](https://img.shields.io/travis/miaolz123/stockdb.svg)](https://travis-ci.org/miaolz123/stockdb)
[![Go Report Card](https://goreportcard.com/badge/github.com/miaolz123/stockdb)](https://goreportcard.com/report/github.com/miaolz123/stockdb)
[![Github All Releases](https://img.shields.io/github/downloads/miaolz123/stockdb/total.svg)](https://github.com/miaolz123/stockdb/releases)
[![Docker Pulls](https://img.shields.io/docker/pulls/stockdb/stockdb.svg)](https://hub.docker.com/r/stockdb/stockdb/)

StockDB 是一个处理交易类数据的数据库解决方案，实现了K线变频、模拟历史市场深度、模拟交易等功能。

# 安装

## 通过 Docker（推荐）

``` shell
$ docker run --name=stockdb -d -p 18765:8765 -v stockdata:/var/lib/influxdb stockdb/stockdb
```

请确保已经安装了 Docker 并可以正常运行，已在 `Docker version 1.12.3` 版本测试通过。

按照示例命令运行成功后 StockDB 将会映射到主机的`http://0.0.0.0:18765`。

<aside class="notice">
通过 Docker 安装的版本已经集成了 InfluxDB 服务，不需要另外的数据库依赖。
</aside>

## 通过二进制文件

1. 下载对应系统的[二进制文件](https://github.com/miaolz123/stockdb/releases/latest)
2. 解压后直接运行

<aside class="warning">
通过二进制文件安装的版本没有集成底层数据库，需要配置额外的 InfluxDB 或 ElasticSerach 服务。
</aside>

## 通过源代码

``` shell
$ git clone https://github.com/miaolz123/stockdb.git
$ cd stockdb
$ go get && go build
```

请确保已经安装了 Golang 运行环境，已在 `go 1.6.2` 版本测试通过。

<aside class="warning">
通过源码安装的版本没有集成底层数据库，需要配置额外的 InfluxDB 或 ElasticSerach 服务。
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
