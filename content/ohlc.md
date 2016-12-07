---
weight: 2
---

# K线数据

## OHLC 结构体

> **OHLC** 结构体示例：

```json
{
  "Time": 1451577600,
  "Open": 123.456,
  "High": 129.123,
  "Low": 110.789,
  "Close": 119.951,
  "Volume": 357.654
}
```

| 字段 | 类型 | 解释 |
| ---- | ---- | ---- |
| Time | Long Integer | UNIX 时间，1970年1月1日0时0分0秒起至现在的总秒数 |
| Open | Double | 单位时间周期内的开盘价 |
| High | Double | 单位时间周期内的最高价 |
| Low | Double | 单位时间周期内的最低价 |
| Close | Double | 单位时间周期内的收盘价 |
| Volume | Double | 单位时间周期内的交易量 |

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

```js
import StockDB from 'stockdb';

const client = StockDB.New('http://localhost:8765', 'username:password');
const data = {
  Time: 1480737600,
  Open: 109.17,
  High: 110.09,
  Low: 108.85,
  Close: 109.90,
  Volume: 26528000
};
const opt = {
  Market: 'nasdaq',
  Symbol: 'aapl',
  Period: StockDB.Day
};

client.PutOHLC(data, opt, (resp) => {
  if (!resp.Success) {
    console.log(`PutOHLC() error: ${resp.Message}`);
  }
}, (func, err) => {
  console.log(`${func}() error: ${err}`);
});
```

`PutOHLC(data, opt)`

| 请求参数 | 类型 | 必填 | 解释 |
| -------- | ---- | ---- | ---- |
| data | [OHLC](#ohlc-结构体) Object | true | 需要新增的数据 |
| opt | Object | true | 操作详细设置 |
| opt.Market | String | false | 添加该数据到哪个市场 |
| opt.Symbol | String | false | 添加该数据到哪个分类标识 |
| opt.Period | Long Integer | false | 该数据的周期，单位为秒 |

<aside class="notice">
如果 <code>opt.Market</code>、<code>opt.Symbol</code>、<code>opt.Period</code> 未设置，将使用 StockDB 服务端的默认配置。
</aside>

<br>

| 响应名称 | 类型 | 解释 |
| -------- | ---- | ---- |
| Success | Boolean | 是否操作成功 |
| Message | String | 操作失败的错误信息 |
| Data | Null | - |

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

```js
import StockDB from 'stockdb';

const client = StockDB.New('http://localhost:8765', 'username:password');
const data = [
  {
    Time: 1480651200,
    Open: 110.37,
    High: 110.94,
    Low: 109.03,
    Close: 109.49,
    Volume: 34324000
  }, {
    Time: 1480737600,
    Open: 109.17,
    High: 110.09,
    Low: 108.85,
    Close: 109.90,
    Volume: 26528000
  }
];
const opt = {
  Market: 'nasdaq',
  Symbol: 'aapl',
  Period: StockDB.Day,
};

client.PutOHLCs(data, opt, (resp) => {
  if (!resp.Success) {
    console.log(`PutOHLCs() error: ${resp.Message}`);
  }
}, (func, err) => {
  console.log(`${func}() error: ${err}`);
});
```

`PutOHLC(data, opt)`

| 请求参数 | 类型 | 必填 | 解释 |
| -------- | ---- | ---- | ---- |
| data | [OHLC](#ohlc-结构体) Object List | true | 需要新增的数据 |
| opt | Object | true | 操作详细设置 |
| opt.Market | String | false | 添加该数据到哪个市场 |
| opt.Symbol | String | false | 添加该数据到哪个分类标识 |
| opt.Period | Long Integer | false | 该数据的周期，单位为秒 |

<aside class="notice">
如果 <code>opt.Market</code>、<code>opt.Symbol</code>、<code>opt.Period</code> 未设置，将使用 StockDB 服务端的默认配置。
</aside>

<br>

| 响应名称 | 类型 | 解释 |
| -------- | ---- | ---- |
| Success | Boolean | 是否操作成功 |
| Message | String | 操作失败的错误信息 |
| Data | Null | - |

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
		Period: stockdb.Day,
	}
	fmt.Printf("Day OHLC:\n%+v\n", client.GetOHLCs(opt).Data)
	opt.Period = stockdb.Week
	fmt.Printf("Week OHLC:\n%+v\n", client.GetOHLCs(opt).Data)
}
```

```js
import StockDB from 'stockdb';

const client = StockDB.New('http://localhost:8765', 'username:password');
const opt = {
  Market: 'nasdaq',
  Symbol: 'aapl',
  Period: StockDB.Day
};

client.GetOHLCs(opt, (resp) => {
  console.log(`Day OHLC: ${resp.Date}`);
}, (func, err) => {
  console.log(`${func}() error: ${err}`);
});
opt.Period = StockDB.Week;
client.GetOHLCs(opt, (resp) => {
  console.log(`Week OHLC: ${resp.Date}`);
}, (func, err) => {
  console.log(`${func}() error: ${err}`);
});
```

`GetOHLCs(opt)`

| 请求参数 | 类型 | 必填 | 解释 |
| -------- | ---- | ---- | ---- |
| opt | Object | true | 操作详细设置 |
| opt.Market | String | false | 市场 |
| opt.Symbol | String | false | 分类标识 |
| opt.Period | Long Integer | false | 周期，单位为秒 |
| opt.BeginTime | Long Integer | false | 开始时间，UNIX 时间 |
| opt.EndTime | Long Integer | false | 开始时间，UNIX 时间 |
| opt.InvalidPolicy | String | false | 无效数据处理策略，默认为`ibid`，`ibid`表示同上一条数据，`ignore`表示忽略 |

<aside class="notice">
如果 <code>opt.Market</code>、<code>opt.Symbol</code>、<code>opt.Period</code> 未设置，将使用 StockDB 服务端的默认配置；
如果 <code>opt.EndTime</code> 小于零，将使用该数据的最新时间；
如果 <code>opt.BeginTime</code> 小于零，将使用 <code>opt.EndTime - (1000 * opt.Period)</code>。
</aside>

<br>

| 响应名称 | 类型 | 解释 |
| -------- | ---- | ---- |
| Success | Boolean | 是否操作成功 |
| Message | String | 操作失败的错误信息 |
| Data | [OHLC](#ohlc-结构体) Object List | 获取的数据 |
