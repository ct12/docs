---
weight: 4
---

# 查询

## 获取 Market

```go
package main

import (
	"fmt"

	"github.com/miaolz123/stockdb/stockdb"
)

func main() {
	client := stockdb.New("http://localhost:8765", "username:password")
	fmt.Printf("All Markets:\n%+v\n", client.GetMarkets().Data)
}
```

```js
import StockDB from 'stockdb';

const client = StockDB.New('http://localhost:8765', 'username:password');

client.GetMarkets((resp) => {
  console.log(`All Markets: ${resp.Date}`);
}, (func, err) => {
  console.log(`${func}() error: ${err}`);
});
```

`GetMarkets()`

| 响应名称 | 类型 | 解释 |
| -------- | ---- | ---- |
| Success | Boolean | 是否操作成功 |
| Message | String | 操作失败的错误信息 |
| Data | String List | 获取的数据 |

## 获取 Symbol

```go
package main

import (
	"fmt"

	"github.com/miaolz123/stockdb/stockdb"
)

func main() {
	client := stockdb.New("http://localhost:8765", "username:password")
	fmt.Printf("All Symbols:\n%+v\n", client.GetSymbols("nasdaq").Data)
}
```

```js
import StockDB from 'stockdb';

const client = StockDB.New('http://localhost:8765', 'username:password');

client.GetSymbols('nasdaq', (resp) => {
  console.log(`All Symbols: ${resp.Date}`);
}, (func, err) => {
  console.log(`${func}() error: ${err}`);
});
```

`GetSymbols(market)`

| 请求参数 | 类型 | 必填 | 解释 |
| -------- | ---- | ---- | ---- |
| market | String | true | 指定一个 Market |

<br>

| 响应名称 | 类型 | 解释 |
| -------- | ---- | ---- |
| Success | Boolean | 是否操作成功 |
| Message | String | 操作失败的错误信息 |
| Data | String List | 获取的数据 |

## 获取K线数据

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

## 获取深度数据

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
		Period: stockdb.Day,
	}
	fmt.Printf("Market Depth:\n%+v\n", client.GetDepth(opt).Data)
}
```

```js
import StockDB from 'stockdb';

const client = StockDB.New('http://localhost:8765', 'username:password');
const opt = {
  Market: 'nasdaq',
  Symbol: 'aapl',
  BeginTime: 1451577600,
  Period: StockDB.Day
};

client.GetDepth(opt, (resp) => {
  console.log(`Market Depth: ${resp.Date}`);
}, (func, err) => {
  console.log(`${func}() error: ${err}`);
});
```

`GetDepth(opt)`

| 请求参数 | 类型 | 必填 | 解释 |
| -------- | ---- | ---- | ---- |
| opt | Object | true | 操作详细设置 |
| opt.Market | String | false | 市场 |
| opt.Symbol | String | false | 分类标识 |
| opt.BeginTime | Long Integer | false | 开始时间，UNIX 时间 |
| opt.Period | Long Integer | false | 周期，单位为秒 |

<aside class="notice">
如果 <code>opt.Market</code>、<code>opt.Symbol</code>、<code>opt.Period</code> 未设置，将使用 StockDB 服务端的默认配置；
如果 <code>opt.BeginTime</code> 小于零，将使用该数据的最新时间。
</aside>

<br>

| 响应名称 | 类型 | 解释 |
| -------- | ---- | ---- |
| Success | Boolean | 是否操作成功 |
| Message | String | 操作失败的错误信息 |
| Data | [OHLC](#ohlc-结构体) Object List | 获取的数据 |

<aside class="warning">
如果获取的深度数据一直为空，建议适当增大 <code>opt.Period</code>。
</aside>
