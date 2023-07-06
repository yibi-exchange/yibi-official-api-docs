<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Market API](#market-api)
  - [GET /v1/summary](#get-v1summary)
  - [GET /v1/assets](#get-v1assets)
  - [GET /v1/ticker](#get-v1ticker)
  - [GET /v1/orderbook/market_pair](#get-v1orderbookmarket_pair)
  - [GET /v1/trades/market_pair](#get-v1tradesmarket_pair)
  - [GET /v1/pairs](#get-v1pairs)
- [Trade API](#trade-api)
  - [GET /v1/user/getBalance](#get-v1usergetbalance)
  - [GET /v1/user/addOrder](#get-v1useraddorder)
  - [GET /v1/user/queryOrder](#get-v1userqueryorder)
  - [GET /v1/user/cancelOrder](#get-v1usercancelorder)
  - [GET /v1/user/queryTrade](#get-v1userquerytrade)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Market API

## GET /v1/summary  
Get the market of all exchange pairs.

Request: None

Url: https://api.yibi.co/v1/summary

Response: Object[]

Name | Data type | Description 
------------ | ------------ | ------------
trading_pairs | string | Identifier of a ticker with delimiter to separate base_target, eg. BTC_USDT
base_currency | string | Symbol/currency code of base pair, eg. BTC
quote_currency | string | Symbol/currency code of target pair, eg. USDT
last_price | decimal | 	Last transacted price of base currency based on given target currency
base_volume | decimal | 24 hour trading volume in base pair volume
quote_volume | decimal | 24 hour trading volume in target pair volume
highest_bid | decimal | Current highest bid price
lowest_ask | decimal | Current lowest ask price
highest_price_24h | decimal | Rolling 24-hours highest transaction price
lowest_price_24h | decimal | Rolling 24-hours lowest transaction price
price_change_percent_24h | decimal | 24-hr % price change of market pair

Data example:
```json
[{
    "trading_pairs": "XRP_BTC",
    "last_price": 0.0000203,
    "lowest_ask": 0.0000213,
    "highest_bid": 0.0000202,
    "base_volume": 350700,
    "quote_volume": 7.139649999999999,
    "price_change_percent_24h": -0.49019607843137253,
    "highest_price_24h": 0.0000204,
    "lowest_price_24h": 0.0000203
  }
]

```

## GET /v1/assets  
Get a detailed summary for each currency available on the exchange.

Request: None

Url: https://api.yibi.co/v1/assets

Response: Object[]

Name | Data type | Description 
------------ | ------------ | ------------
name | string | Full name of cryptocurrency.
symbol | string | currency code, eg. BTC
unified_cryptoasset_id | integer | Unified cryptoasset id.
can_withdraw | boolean | Identifies whether withdrawals are enabled or disabled.
can_deposit | boolean | 	Identifies whether deposits are enabled or disabled.
min_withdraw | decimal | Identifies the single minimum withdrawal amount of a cryptocurrency.
max_withdraw | decimal | Identifies the single maximum withdrawal amount of a cryptocurrency.
maker_fee | decimal | Fees applied when liquidity is added to the order book.
taker_fee | decimal | Fees applied when liquidity is removed from the order book.
contractAddressUrl | string | URL of contract address on each chain.
contractAddress | string | Contract address of the asset on each chain.
                    
Data example:
```json
{  
   "BTC":{  
      "name":"bitcoin",
      "unified_cryptoasset_id" :"1",
      "can_withdraw":"true",
      "can_deposit":"true",
      "min_withdraw":"0.01",
      "max_withdraw ":"100" 
      "name":"bitcoin",
      "maker_fee":"0.01",
      "taker_fee":"0.01",
   },
   "ETH":{  
      "name":"arbitrum",
      "unified_cryptoasset_id":"11841",
      "can_withdraw":"false",
      "can_deposit":"false",
      "min_withdraw":"10.00",
      "max_withdraw ":"0.00" 
      "maker_fee":"0.01",
      "taker_fee":"0.01",
      "contractAddressUrl": "https://arbiscan.io/token/0x912CE59144191C1204E64559FE8253a0e49E6548",
      "contractAddress": "0x912CE59144191C1204E64559FE8253a0e49E6548"
   }
}

```

## GET /v1/ticker  
Get a 24-hour pricing and volume summary for each market pair available on the exchange.

Request: None

Url: https://api.yibi.co/v1/ticker

Response: Object[]

Name | Data type | Description 
------------ | ------------ | ------------
base_id | integer | The quote pair Unified Cryptoasset ID.
quote_id | integer | The base pair Unified Cryptoasset ID.
last_price | decimal | Last transacted price of base currency based on given target currency
base_volume | decimal | 24-hour trading volume denoted in BASE currency
quote_volume | decimal | 24 hour trading volume denoted in TARGET currency
isFrozen | integer | Indicates if the market is currently enabled (0) or disabled (1).
                      
Data example:
```json
{  
   "BTC_USDT":{  
      "base_id":"1",
      "quote_id":"825",
      "last_price":"10000",
      "quote_volume":"20000",
      "base_volume":"2",   
      "isFrozen":"0"
   },
   "LTC_BTC":{  
      "base_id":"2",
      "quote_id":"1",
      "last_price":"0.00699900",
      "base_volume":"20028,526",
      "quote_volume":"279594",
      "isFrozen":"0"
   },
"BNB_BTC":{  
      "base_id":"1839",
      "quote_id":"1",
      "last_price":"0.00699900",
      "base_volume":"53819",
      "quote_volume":"99.3459",      
      "isFrozen":"0"
   }
}

```

## GET /v1/orderbook/market_pair

Order book depth details

Request:  

Name | Data type | Description 
------------ | ------------ | ------------
market_pair | string | A pair such as "BTC_USDT"
depth | integer | (optional)Order depth quantity：[100, 200, 500...]. Depth = 100 means 50 for each buy/sell.

Url: https://api.yibi.co/v1/orderbook/BTC_USDT

Response: Object[]

Name | Data type | Description 
------------ | ------------ | ------------
timestamp | timestamp | Unix timestamp in milliseconds for when the last updated time occurred.
bids | decimal | An array containing 2 elements. The offer price and quantity for each bid order
asks | decimal | An array containing 2 elements. The ask price and quantity for each ask order
tickerId | string | Identifier of a ticker with delimiter to separate base_target, eg. BTC_USDT


Data example:
```
/* GET /v1/orderbook/BTC_USDT */
```
```json
{
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
```

## GET /v1/trades/market_pair

Transaction records

Request:  

Name | Data type | Description 
------------ | ------------ | ------------
market_pair | String | A pair such as "BTC_USDT"

Url: https://api.yibi.co/v1/trades/BTC_USDT

Response: Object

Name | Data type | Description 
------------ | ------------ | ------------
trade_id | int | Records id
price | decimal | Transaction price in base pair volume.
base_volume | decimal | Transaction amount in base pair volume.
quote_volume | decimal | Transaction amount in target pair volume.
timestamp | timestamp | Unix timestamp in milliseconds for when the transaction occurred.
type | string | Used to determine the type of the transaction that was completed. buy – Identifies an ask that was removed from the order book. sell – Identifies a bid that was removed from the order book.
                                                                                                
Data example:
```
/* GET /v1/historicalTrades?tickerId=BTC_USDT */
```
```json
[   
   {         
      "trade_id":3523643,
      "price":"0.01",
      "base_volume":"569000",
      "quote_volume":"0.01000000",
      "timestamp":"‭1585177482652‬",
      "type":"sell"
   }
]

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
type | integer |  1 buy, 2 sell
price | string | Order price
qty | string | Order quantity


Response: Object

Name | Data type | Description 
------------ | ------------ | ------------
data | long | Order id

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
pageIndex | integer | Page Num (Default 1)
pageSize | integer | Page size (Default 20; max 100.)
status | string | (optional)Status: -1 Canceled, 0 Ordered, 1 Wait match, 2 Matching, 3 Matching completed, 4 Currrent Open Order
tickerId | string | (optional)A pair such as "BTC_USDT"


Response: Object

Name | Data type | Description 
------------ | ------------ | ------------
id | long | Order id
symbol | string | A pair such as "BTCUSDT"
type | integer | Order type  1 buy, 2 sell
price | decimal | Order price
qty | decimal | Order quantity
tradeQty | decimal | Traded quantity
status | integer | Status: -1 Canceled, 0 Ordered, 1 Wait match, 2 Matching, 3 Matching completed
createTime | long | Order time


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
          "status": 1,
          "symbol": "BTCUSDT"
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

## GET /v1/user/queryTrade

Query trade records

Request: 

Name | Data type | Description 
------------ | ------------ | ------------
apiKey | string | API_KEY applied by the user
sign | string | Parameter signature
pageIndex | integer | Page Num (Default 1)
pageSize | integer | Page size (Default 20; max 100.)
tickerId | string | A pair such as "BTC_USDT"
orderId | long | (optional)order id



Response: Object

Name | Data type | Description 
------------ | ------------ | ------------
id | integer | Trade id
orderId | long | Order id
symbol | string | A pair such as "BTCUSDT"
orderType | integer | Order type  1 Buy, 2 Sell, 3 Market buy, 4 Market sell
price | decimal | Trade price
qty | decimal | Trade quantity
quoteQty | decimal | Trade quoteQty
fee | decimal | Trade fee
feeAsset | string | Trade fee asset
isMaker | integer | 1 MAKER, 2 TAKER
createTime | long | Trade time


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
          "orderId": 222555312,
          "orderType": 1,
          "quoteQty": 0,
          "fee": 0.0001,
          "feeAsset": "BTC",
          "isMaker": 1,
          "symbol": "BTCUSDT"
      }
  ]
}
```
