<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [行情API](#%E8%A1%8C%E6%83%85api)
  - [GET /v1/summary](#get-v1summary)
  - [GET /v1/assets](#get-v1assets)
  - [GET /v1/ticker](#get-v1ticker)
  - [GET /v1/orderBook](#get-v1orderbook)
  - [GET /v1/historicalTrades](#get-v1historicaltrades)
  - [GET /v1/pairs](#get-v1pairs)
- [交易API](#%E4%BA%A4%E6%98%93api)
  - [GET /v1/user/getBalance](#get-v1usergetbalance)
  - [GET /v1/user/addOrder](#get-v1useraddorder)
  - [GET /v1/user/queryOrder](#get-v1userqueryorder)
  - [GET /v1/user/cancelOrder](#get-v1usercancelorder)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# 行情API

## GET /v1/summary
获取所有交易对信息

请求参数:  无

返回参数: Array，结构如下  

参数名 | 类型 | 描述 
------------ | ------------ | ------------
tickerId | string | 交易对名称
baseCurrency | string | 交易币种
targetCurrency | string | 市场币种
lastPrice | decimal | 	最新价格
baseVolume | decimal | 24小时内交易币成交数量
targetVolume | decimal | 24小时内成交额
bid | decimal | 买一价格
ask | decimal | 卖一价格
high | decimal | 24小时内最高成交价格
low | decimal | 24小时内最低成交价格

示例数据：
```json
{
"code": "1",
"success": true,
"msg": null,
"data": [{
    "tickerId": "BTC_USDT",
    "baseCurrency": "BTC",
    "targetCurrency": "USDT",
    "lastPrice": "48527.91",
    "baseVolume": "229.852284",
    "targetVolume": "11212881.13491958",
    "bid": "48735.03",
    "ask": "45799.32",
    "high": "49047.4",
    "low": "48493.24"
    }]
}
```

## GET /v1/assets  
获取交易所可用的每种货币的详细摘要

请求参数:  无

返回参数: Array，结构如下  

参数名 | 类型 | 描述 
------------ | ------------ | ------------
name | string | 币种全称
symbol | string | 币种名称
unifiedCryptoAssetId | integer | 唯一币种id
canWithdraw | boolean | 是否允许提币
canDeposit | boolean | 	是否允许充值
minWithdraw | decimal | 最低提币数量限额
maxWithdraw | decimal | 最高提币数量限额
maker_fee | decimal | 买入手续费率
taker_fee | decimal | 卖出手续费率
                      
示例数据：
```json
{
    "code":"1",
    "success":true,
    "msg":null,
    "data":[
        {
            "name":"Ethereum",
            "unifiedCryptoAssetId":"1027",
            "canWithdraw":"true",
            "canDeposit":"true",
            "minWithdraw":0.1,
            "maxWithdraw":20,
            "symbol":"ETH",
            "makerFee":0.002,
            "takerFee":0.002
        },
        {
            "name":"Bitcoin",
            "unifiedCryptoAssetId":"1",
            "canWithdraw":"true",
            "canDeposit":"true",
            "minWithdraw":0.0006,
            "maxWithdraw":2,
            "symbol":"BTC",
            "makerFee":0.002,
            "takerFee":0.002
        }
    ]
}
```

## GET /v1/ticker  
获取交易所上每个可用市场对的 24 小时定价和交易量数据。

请求参数:  无

返回参数: Array，结构如下  

参数名 | 类型 | 描述 
------------ | ------------ | ------------
tickerId | string | 交易对名称
lastPrice | decimal | 最后成交价格
baseVolume | decimal | 24小时内交易币成交数量
targetVolume | decimal | 24小时内成交额
isFrozen | integer | 0 交易对开启 1交易对下架
                      
Data example:
```json
{
    "code":"1",
    "success":true,
    "msg":null,
    "data":[
        {
            "tickerId":"BTC_USDT",
            "lastPrice":"42065.72",
            "baseVolume":"1084.92",
            "targetVolume":"45448520.44",
            "isFrozen":0
        },
        {
            "tickerId":"ETH_USDT",
            "lastPrice":"3147.16",
            "baseVolume":"9659.14",
            "targetVolume":"30085798.27",
            "isFrozen":0
        }
    ]
}
```

## GET /v1/orderBook

获取深度

请求参数:  

参数名 | 类型 | 描述 |
------------ | ------------ | ------------
tickerId | string | 交易对名称 币种名称后面需要加"_"和板块名称(BTC_USDT)
depth | integer | （可选）订单深度数量：[100, 200, 500...]。 深度 = 100 表示每个买/卖方为 50。

返回参数: Object数组，结构如下 

参数名 | 类型 | 描述 
------------ | ------------ | ------------
tickerId | string | 交易对名称
timestamp | timestamp | 更新时间
bids | decimal | 包含 2 个元素的数组。 每个订单的报价和数量
asks | decimal | 包含 2 个元素的数组。 每个订单的报价和数量



示例数据：
```
/* GET /v1/orderBook?tickerId=BTC_USDT */
```
```json
{
"code": "1",
"success": true,
"msg": null,
"data": {
    "tickerId": "BTC_USDT",
    "timestamp": "1639473000000",
    "bids": [
        ["48735.05", "0.200128"],
        ["48735.04", "0.1005"]
  ],
    "asks": [
        ["48735.06", "0.203296"],
        ["48735.07", "0.108152"]
     ]
  }
}
```

## GET /v1/historicalTrades

获取成交记录

请求参数:  

参数名 | 类型 | 描述 
------------ | ------------ | ------------
tickerId | String | 市场名称 币种名称后面需要加"_"和板块名称(BTC_USDT)


返回参数: Object，结构如下  


参数名 | 类型 | 描述 
------------ | ------------ | ------------
tradeId | int | 成交记录id
price | decimal | 交易价格
base_volume | decimal | 成交数量
target_volume | decimal | 成交额
trade_timestamp | timestamp | 成交时间，时间戳
type | string | buy 是买入，sell是卖出

示例数据：
```
/* GET /v1/historicalTrades?tickerId=BTC_USDT */
```
```json
{
"code": "1",
"success": true,
"msg": null,
"data": {
    "buy": [{
        "trade_id": 187317363,
        "price": 293392.99,
        "base_volume": 0.000146,
        "target_volume": 42.83537654,
        "trade_timestamp": 1639470864000,
        "type": "buy"
}],
    "sell": [{
        "trade_id": 187314280,
        "price": 297323.2,
        "base_volume": 0.45152,
        "target_volume": 134247.37126400,
        "trade_timestamp": 1639470624000,
        "type": "sell"
    }]
  }
}
```

## GET /v1/pairs  
获取所有交易对列表

请求参数： 无  

返回参数： Array，结构如下  

参数名 | 类型 | 描述 
------------ | ------------ | ------------
tickerId | string | 交易对名称
base | string | 交易币种
target | string | 市场币种


示例数据：
```json
{
"code": "1",
"success": true,
"msg": null,
"data": [{
    "tickerId": "ETH_USDT",
    "base": "ETH",
    "target": "USDT"
}, {
    "tickerId": "BTC_USDT",
    "base": "BTC",
    "target": "USDT"
  }]
}
```

# 交易API

## GET /v1/user/getBalance

获取获取用户所有余额

请求参数:  

参数名 | 类型 | 描述 
------------ | ------------ | ------------
apiKey | string | 用户申请的API_KEY
sign | string | 参数签名

返回参数: Array，结构如下  

参数名 | 类型 | 描述 
------------ | ------------ | ------------
asset | string | 数字货币名称
free | decimal | 可用余额
locked | decimal | 冻结余额

示例数据：
```
/* GET /v1/user/getBalance?apiKey=1&sign=2 */
```
```json
{
"code": "1",
"success": true,
"msg": null,
"data": [{
    "asset": "BTC",
    "free": "0",
    "locked": "0"
    }]
}
```

## GET /v1/user/addOrder

现货交易下单

请求参数:  

参数名 | 类型 | 描述 
------------ | ------------ | ------------
apiKey | string | 用户申请的API_KEY
sign | string | 参数签名
tickerId | string | 市场名称 币种名称后面需要加"_"和板块名称(BTC_USDT)
type | string | 类型 1买入 2卖出
price | string | 价格
qty | string | 数量


返回参数: Object，结构如下  

参数名 | 类型 | 描述 
------------ | ------------ | ------------
data | integer | 订单id

示例数据：
```json
{
  "code": "1",
  "success": true,
  "data": 10000001
}
```

## GET /v1/user/queryOrder

查询币币订单

请求参数:  

参数名 | 类型 | 描述 
------------ | ------------ | ------------
apiKey | string | 用户申请的API_KEY
sign | string | 参数签名
status | string | （可选）状态 -1已撤单 0已下单 1待撮合 2撮合中 3已完成撮合
tickerId | string | （可选）市场名称 币种名称后面需要加"_"和板块名称(BTC_USDT)


返回参数: Object，结构如下  

参数名 | 类型 | 描述 
------------ | ------------ | ------------
id | integer | 订单id
type | integer | 订单类型  1buy 2sell
price | decimal | 价格
qty | decimal | 下单数量
tradeQty | decimal | 已成交数量
status | integer | 状态-1已撤单 0已下单 1待撮合 2撮合中 3已完成


示例数据：
```json
{
  "code": "1",
  "success": true,
  "msg": null,
  "data": [
    {
      "createTime": 1534301500000,
      "price": 0.045662,
      "qty": 0.253,
      "id": 1440592,
      "type": 1,
      "tradeQty": 0,
      "status": 1
  }]
}
```

## GET /v1/user/cancelOrder

撤单

请求参数:  

参数名 | 类型 | 描述 
------------ | ------------ | ------------
apiKey | string | 用户申请的API_KEY
sign | string | 参数签名
id | integer | 订单id

示例数据：
```json
{
  "code": "1",
  "success": true,
  "msg": "撤单成功"
}
```
