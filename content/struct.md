---
weight: 2
---

# 模型

## 基本概念

为方便理解 StockDB 的一些概念，下面做一些类比：

| StockDB | MySQL | InfluxDB | ELasticSearch | 解释 | 示例 |
| ------- | ----- | -------- | ------------- | ---- | ---- |
| Market | Database | Database | Index | 数据所属的市场 | `nasdaq`，`上证` |
| Symbol | Tabel | Measurement | Type | 数据的分类标识 | `aapl`，`平安银行` |

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

## OrderBook 结构体

> **OrderBook** 结构体示例：

```json
{
  "Price": 123.456,
  "Amount": 789.01
}
```

| 字段 | 类型 | 解释 |
| ---- | ---- | ---- |
| Price | Double | 报价 |
| Amount | Double | 数量 |

## Depth 结构体

> **Depth** 结构体示例：

```json
{
  "Bids": [
    {
      "Price": 122.456,
      "Amount": 789.01
    },
    {
      "Price": 121.455,
      "Amount": 789.01
    }
  ],
  "Asks": [
    {
      "Price": 124.456,
      "Amount": 789.01
    },
    {
      "Price": 125.457,
      "Amount": 789.01
    }
  ]
}
```

| 字段 | 类型 | 解释 |
| ---- | ---- | ---- |
| Bids | [OrderBook](#orderbook-结构体) Object List | 买盘市场深度，按照价格**降序**排列 |
| Asks | [OrderBook](#orderbook-结构体) Object List | 卖盘市场深度，按照价格**升序**排列 |
