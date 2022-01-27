<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Market API](#market-api)
  - [GET /v1/summary](#get-v1summary)
  - [GET /v1/assets](#get-v1assets)
  - [GET /v1/ticker](#get-v1ticker)
  - [GET /v1/orderBook](#get-v1orderbook)
  - [GET /v1/historicalTrades](#get-v1historicaltrades)
  - [GET /v1/pairs](#get-v1pairs)
- [Trade API](#trade-api)
  - [GET /v1/user/getBalance](#get-v1usergetbalance)
  - [GET /v1/user/addOrder](#get-v1useraddorder)
  - [GET /v1/user/queryOrder](#get-v1userqueryorder)
  - [GET /v1/user/cancelOrder](#get-v1usercancelorder)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Market API

## GET /v1/summary  
Get the market of all exchange pairs.

Request: None

Response: Object[]

Name | Data type | Description 
------------ | ------------ | ------------
tickerId | string | Identifier of a ticker with delimiter to separate base_target, eg. BTC_USDT
baseCurrency | string | Symbol/currency code of base pair, eg. BTC
targetCurrency | string | Symbol/currency code of target pair, eg. USDT
lastPrice | decimal | 	Last transacted price of base currency based on given target currency
baseVolume | decimal | 24 hour trading volume in base pair volume
targetVolume | decimal | 24 hour trading volume in target pair volume
bid | decimal | Current highest bid price
ask | decimal | Current lowest ask price
high | decimal | Rolling 24-hours highest transaction price
low | decimal | Rolling 24-hours lowest transaction price

Data example:
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
Get a detailed summary for each currency available on the exchange.

Request: None

Response: Object[]

Name | Data type | Description 
------------ | ------------ | ------------
name | string | Full name of cryptocurrency.
symbol | string | currency code, eg. BTC
unifiedCryptoAssetId | integer | Unified cryptoasset id.
canWithdraw | boolean | Identifies whether withdrawals are enabled or disabled.
canDeposit | boolean | 	Identifies whether deposits are enabled or disabled.
minWithdraw | decimal | Identifies the single minimum withdrawal amount of a cryptocurrency.
maxWithdraw | decimal | Identifies the single maximum withdrawal amount of a cryptocurrency.
maker_fee | decimal | Fees applied when liquidity is added to the order book.
taker_fee | decimal | Fees applied when liquidity is removed from the order book.
                      
Data example:
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
Get a 24-hour pricing and volume summary for each market pair available on the exchange.

Request: None

Response: Object[]

Name | Data type | Description 
------------ | ------------ | ------------
tickerId | string | Identifier of a ticker with delimiter to separate base_target, eg. BTC_USDT.
lastPrice | decimal | Last transacted price of base currency based on given target currency
baseVolume | decimal | 24-hour trading volume denoted in BASE currency
targetVolume | decimal | 24 hour trading volume denoted in TARGET currency
isFrozen | integer | Indicates if the market is currently enabled (0) or disabled (1).
                      
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

Order book depth details

Request:  

Name | Data type | Description 
------------ | ------------ | ------------
tickerId | string | A pair such as "BTC_USDT"
depth | integer | (optional)Order depth quantity：[100, 200, 500...]. Depth = 100 means 50 for each buy/sell.

Response: Object[]

Name | Data type | Description 
------------ | ------------ | ------------
tickerId | string | A pair such as "BTC_USDT"
timestamp | timestamp | Unix timestamp in milliseconds for when the last updated time occurred.
bids | decimal | An array containing 2 elements. The offer price and quantity for each bid order
asks | decimal | An array containing 2 elements. The ask price and quantity for each ask order



Data example:
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

Transaction records

Request:  

Name | Data type | Description 
------------ | ------------ | ------------
tickerId | String | A pair such as "BTC_USDT"

Response: Object

Name | Data type | Description 
------------ | ------------ | ------------
tradeId | int | Records id
price | decimal | Transaction price in base pair volume.
base_volume | decimal | Transaction amount in base pair volume.
target_volume | decimal | Transaction amount in target pair volume.
trade_timestamp | timestamp | Unix timestamp in milliseconds for when the transaction occurred.
type | string | Used to determine the type of the transaction that was completed. Buy – Identifies an ask that was removed from the order book. Sell – Identifies a bid that was removed from the order book.
                                                                                                
Data example:
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
Get the list of all exchange pairs.

Request: None

Response: Object[]

Name | Data type | Description 
------------ | ------------ | ------------
tickerId | string | Exchange pair name
base | string | Symbol/currency code of a the base crypto asset, eg. BTC
target | string | Symbol/currency code of the target crypto asset, eg. USDT


Data example:
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

# Trade API

## GET /v1/user/getBalance

Get Get all the balance of the user

Request:  

Name | Data type | Description 
------------ | ------------ | ------------
apiKey | string | API_KEY applied by the user
sign | string | Parameter signature

Response: Object[]

Name | Data type | Description 
------------ | ------------ | ------------
asset | string | Currency name
free | decimal | Available Balance
locked | decimal | Freeze balance

Data example:
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

Spot trading order

Request:   

Name | Data type | Description 
------------ | ------------ | ------------
apiKey | string | API_KEY applied by the user
sign | string | Parameter signature
tickerId | string | A pair such as "BTC_USDT"
type | string |  1 buy, 2 sell
price | string | Order price
qty | string | Order quantity


Response: Object

Name | Data type | Description 
------------ | ------------ | ------------
data | integer | Order id

Data example:
```json
{
  "code": "1",
  "success": true,
  "data": 11111
}
```

## GET /v1/user/queryOrder

Query order records

Request: 

Name | Data type | Description 
------------ | ------------ | ------------
apiKey | string | API_KEY applied by the user
sign | string | Parameter signature
status | string | (optional)Status: -1 canceled, 0 ordered, 1 wait match, 2 Matching, 3 Matching completed
tickerId | string | (optional)A pair such as "BTC_USDT"

Response: Object

Name | Data type | Description 
------------ | ------------ | ------------
id | integer | Order id
type | integer | Order type  1 buy, 2 sell
price | decimal | Order price
qty | decimal | Order quantity
tradeQty | decimal | Traded quantity
status | integer | Status: -1 canceled, 0 ordered, 1 wait match, 2 Matching, 3 Matching completed


Data example:
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
      }
  ]
}
```

## GET /v1/user/cancelOrder

Cancel order

Request:

Name | Data type | Description 
------------ | ------------ | ------------
apiKey | string | API_KEY applied by the user
sign | string | Parameter signature
id | integer | Order id

Data example:
```json
{
  "code": "1",
  "success": true,
  "msg": "cancel success"
}
```
