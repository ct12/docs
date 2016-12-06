---
weight: 2
---

# K线数据

## OHLC 结构体

> **OHLC** 结构体示例：

```json
{
  "time": 1451577600,
  "open": 123.456,
  "high": 129.123,
  "low": 110.789,
  "close": 119.951,
  "volume": 357.654
}
```

| 字段 | 类型 | 解释 |
| ---- | ---- | ---- |
| time | Long Integer | UNIX 时间，1970年1月1日0时0分0秒起至现在的总秒数 |
| open | Double | 单位时间周期内的开盘价 |
| high | Double | 单位时间周期内的最高价 |
| low | Double | 单位时间周期内的最低价 |
| close | Double | 单位时间周期内的收盘价 |
| volume | Double | 单位时间周期内的交易量 |

## 添加一条数据

```go
package main

import (
	"log"

	"github.com/miaolz123/stockdb/stockdb"
)

func main() {
	client := stockdb.New("http://localhost:8765", "username:password")
	data := stockdb.OHLC{
		Time:   1480737600,
		Open:   109.17,
		High:   110.09,
		Low:    108.85,
		Close:  109.90,
		Volume: 26528000.0,
	}
	opt := stockdb.Option{
		Market: "nasdaq",
		Symbol: "aapl",
		Period: stockdb.Day,
	}
	if resp := client.PutOHLC(data, opt); !resp.Success {
		log.Println("PutOHLC() error: ", resp.Message)
	}
}
```

`PutOHLC(data, opt)`

| 请求参数 | 类型 | 必填 | 解释 |
| -------- | ---- | ---- | ---- |
| data | [OHLC](#ohlc-结构体) Object | true | 需要新增的数据 |
| opt | Object | true | 操作详细设置 |
| opt.market | String | false | 添加该数据到哪个市场 |
| opt.symbol | String | false | 添加该数据到哪个分类标识 |
| opt.period | Long Integer | false | 该数据的周期，单位为秒 |

<aside class="notice">
如果 <code>opt.market</code>、<code>opt.symbol</code>、<code>opt.period</code> 未设置，将使用 StockDB 服务端的默认配置。
</aside>

<br>

| 响应名称 | 类型 | 解释 |
| -------- | ---- | ---- |
| success | Boolean | 是否操作成功 |
| message | String | 操作失败的错误信息 |
| data | Null | - |

## 添加多条数据

```go
package main

import (
	"log"

	"github.com/miaolz123/stockdb/stockdb"
)

func main() {
	client := stockdb.New("http://localhost:8765", "username:password")
	data := []stockdb.OHLC{{
		Time:   1480651200,
		Open:   110.37,
		High:   110.94,
		Low:    109.03,
		Close:  109.49,
		Volume: 34324000.0,
	}, {
		Time:   1480737600,
		Open:   109.17,
		High:   110.09,
		Low:    108.85,
		Close:  109.90,
		Volume: 26528000.0,
	}}
	opt := stockdb.Option{
		Market: "nasdaq",
		Symbol: "aapl",
		Period: stockdb.Day,
	}
	if resp := client.PutOHLCs(data, opt); !resp.Success {
		log.Println("PutOHLCs() error: ", resp.Message)
	}
}
```

`PutOHLC(data, opt)`

| 请求参数 | 类型 | 必填 | 解释 |
| -------- | ---- | ---- | ---- |
| data | [OHLC](#ohlc-结构体) Object List | true | 需要新增的数据 |
| opt | Object | true | 操作详细设置 |
| opt.market | String | false | 添加该数据到哪个市场 |
| opt.symbol | String | false | 添加该数据到哪个分类标识 |
| opt.period | Long Integer | false | 该数据的周期，单位为秒 |

<aside class="notice">
如果 <code>opt.market</code>、<code>opt.symbol</code>、<code>opt.period</code> 未设置，将使用 StockDB 服务端的默认配置。
</aside>

<br>

| 响应名称 | 类型 | 解释 |
| -------- | ---- | ---- |
| success | Boolean | 是否操作成功 |
| message | String | 操作失败的错误信息 |
| data | Null | - |

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
		Period: stockdb.Hour,
	}
	fmt.Printf("Hour OHLC:\n%+v\n", client.GetOHLCs(opt).Data)
	opt.Period = stockdb.Week
	fmt.Printf("Week OHLC:\n%+v\n", client.GetOHLCs(opt).Data)
}
```

`GetOHLCs(opt)`

| 请求参数 | 类型 | 必填 | 解释 |
| -------- | ---- | ---- | ---- |
| opt | Object | true | 操作详细设置 |
| opt.market | String | false | 市场 |
| opt.symbol | String | false | 分类标识 |
| opt.period | Long Integer | false | 周期，单位为秒 |
| opt.beginTime | Long Integer | false | 开始时间，UNIX 时间 |
| opt.endTime | Long Integer | false | 开始时间，UNIX 时间 |
| opt.invalidPolicy | String | false | 无效数据处理策略，默认为`ibid`，`ibid`表示同上一条数据，`ignore`表示忽略 |

<aside class="notice">
如果 <code>opt.market</code>、<code>opt.symbol</code>、<code>opt.period</code> 未设置，将使用 StockDB 服务端的默认配置；
如果 <code>opt.endTime</code> 小于零，将使用该数据的最新时间；
如果 <code>opt.beginTime</code> 小于零，将使用 <code>opt.endTime - (1000 * opt.period)</code>。
</aside>

<br>

| 响应名称 | 类型 | 解释 |
| -------- | ---- | ---- |
| success | Boolean | 是否操作成功 |
| message | String | 操作失败的错误信息 |
| data | [OHLC](#ohlc-结构体) Object List | 获取的数据 |
