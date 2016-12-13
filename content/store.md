---
weight: 3
---

# 存储

## 添加一条K线数据

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

## 添加多条K线数据

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
