---
weight: 3
---

# 市场深度

## OrderBook 结构体

> **OrderBook** 结构体示例：

```json
{
  "price": 123.456,
  "amount": 789.01
}
```

| 字段 | 类型 | 解释 |
| ---- | ---- | ---- |
| price | Double | 报价 |
| amount | Double | 数量 |

## Depth 结构体

> **Depth** 结构体示例：

```json
{
  "bids": [
    {
      "price": 122.456,
      "amount": 789.01
    },
    {
      "price": 121.456,
      "amount": 789.01
    }
  ],
  "asks": [
    {
      "price": 124.456,
      "amount": 789.01
    },
    {
      "price": 125.456,
      "amount": 789.01
    }
  ]
}
```

| 字段 | 类型 | 解释 |
| ---- | ---- | ---- |
| bids | [OrderBook](#orderbook-结构体) Object List | 买盘市场深度，按照价格**降序**排列 |
| asks | [OrderBook](#orderbook-结构体) Object List | 卖盘市场深度，按照价格**升序**排列 |

## 获取数据

```go
package main

import (
	"fmt"

	"github.com/miaolz123/stockdb/stockdb"
)

func main() {
	client := stockdb.New("http://localhost:8765", "username:password")
	opt := stockdb.Option{
		Market: "nasdaq",
		Symbol: "aapl",
		BeginTime: 1451577600,
		Period: stockdb.Hour,
	}
	fmt.Printf("Market Depth:\n%+v\n", client.GetDepth(opt).Data)
}
```

`GetDepth(opt)`

| 请求参数 | 类型 | 必填 | 解释 |
| -------- | ---- | ---- | ---- |
| opt | Object | true | 操作详细设置 |
| opt.market | String | false | 市场 |
| opt.symbol | String | false | 分类标识 |
| opt.beginTime | Long Integer | false | 开始时间，UNIX 时间 |
| opt.period | Long Integer | false | 周期，单位为秒 |

<aside class="notice">
如果 <code>opt.market</code>、<code>opt.symbol</code>、<code>opt.period</code> 未设置，将使用 StockDB 服务端的默认配置；
如果 <code>opt.beginTime</code> 小于零，将使用该数据的最新时间。
</aside>

<br>

| 响应名称 | 类型 | 解释 |
| -------- | ---- | ---- |
| success | Boolean | 是否操作成功 |
| message | String | 操作失败的错误信息 |
| data | [OHLC](#ohlc-结构体) Object List | 获取的数据 |

<aside class="warning">
如果获取的深度数据一直为空，建议适当增大 <code>opt.period</code>。
</aside>
