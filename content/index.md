---
weight: 1
---

# 简介

```go
package main

import (
	"github.com/miaolz123/stockdb/stockdb"
)

func main() {
	client := stockdb.New("http://localhost:8765", "username:password")
	// balabala
}
```

> 注意：把`"http://localhost:8765"`和`"username:password"`自己的 StockDB 服务端地址和验证。

StockDB 是一个处理交易类数据的数据库解决方案，实现了K线变频、模拟历史市场深度、模拟交易等功能。

对外提供多个编程语言的API接口，支持：

- JavaScript（官方）
- Golang（官方）
- Python
- Java
- Ruby
- PHP
- C++
- C#

# 基本概念

为方便理解 StockDB 的一些概念，下面做一些类比：

| StockDB | MySQL | InfluxDB | ELasticSearch | 示例 |
| ------- | ----- | -------- | ------------- | ------- |
| Market | Database | Database | Index | `nasdaq`，`上证` |
| Symbol | Tabel | Measurement | Type | `aapl`，`平安银行` |
