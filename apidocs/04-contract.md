# Contract

### Terminology

**side**

The side of the trade.

`BUY_OPEN`: open a long position. (Hedge Mode)

`SELL_CLOSE`: close a long position. (Hedge Mode)

`SELL_OPEN`: open a short position. (Hedge Mode)

`BUY_CLOSE`: close a short position. (Hedge Mode)

`BUY`: increase long position or decrease short position. (Oneway Mode)

`SELL`: increase short position or decrease long position. (Oneway Mode)

**priceType**

Price types.

`INPUT`: The system will use the price you entered exactly to fill the orders.

`MARKET`: The price will be newest price * (1 ± 3%).

For example, if you are opening 10 contracts long, the latest price is 10. Then you will be sending out an order buying 10 contracts at (10 * 1.03)=10.3.

**timeInForce**

Time in force.

`GTC`: Good till canceled. Meaning the order will stand unless otherwise cancelled.

`IOC`: Immediate or cancel. Meaning the order will be cancelled if not executed immediately. Recommended if you want to fill the entire order immediately.

`FOK`: Fill or kill. Meaning the order will be canceled if not immediately filled. Recommended if you want to fill as much as possible, but not necessarily all of, the order immediately.


**orderType**

Order type.

`LIMIT`: Orders to be executed given a specified price or better.

`STOP`: Order that will be triggered once it reaches the `triggerPrice`.



**isPostOnly** (true, false)

This option is used for Post Only type orders. Order will be cancelled if executed immediately.

**isReduceOnly** (true, false)

Orders with this option are only used to reduce positions.
(This option is only supported in Oneway Mode)

**price**

price of order.

**quantity**

contracts qty of order


**Rate limiters (rateLimitType)**

* REQUESTS_WEIGHT
* ORDERS

**Rate limit intervals**

* SECOND
* MINUTE
* DAY

For example:
```json
{
  "rateLimitType": "ORDERS",
  "interval": "SECOND",
  "limit": 20
}
```

---

### Public

Security Type: **None**

Endpoints under **Public** section can be accessed freely without requiring any API-key or signatures.

#### Get broker trading information (contracts)

```shell
GET /openapi/v1/brokerInfo
```

Get Current contract trading rules

**Weight:**
0

**Parameters：**

| name   | type   | required | default | description                                                                                                                                                       |
|--------|--------|----------|---------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `type` | string | `NO`     |         | Trading section information. Possible values include `token`, `options`, and `contracts`. If the parameter is not sent, all trading information will be returned. |

**Response:**

| name         | type   | example         | description                                   |
|--------------|--------|-----------------|-----------------------------------------------|
| `timezone`   | string | `UTC`           | Timezone of timestamp                         |
| `serverTime` | long   | `1554887652929` | Retrieves the current time on server (in ms). |

In the `rateLimits` field: Order api request limit will be displayed.

| name            | type   | example  | description                           |
|-----------------|--------|----------|---------------------------------------|
| `rateLimitType` | string | `ORDERS` | Rate Limit type                       |
| `interval`      | string | `SECOND` | Rate Limit interval                   |
| `limit`         | string | `1500`   | Rate Limit value within the interval. |

In the `contracts` field:
All actively trading contracts will be displayed.

| name                  | type   | example   | description                                                                                                                                              |
|-----------------------|--------|-----------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| `symbol`              | string | `BTCUSD`  | Name of the contract.                                                                                                                                    |
| `status`              | string | `TRADING` | Status of the contract.                                                                                                                                  |
| `baseAsset`           | string | `BTCUSD`  | Base Asset of the trading pair. In the case with contracts, the contract itself is the base asset.                                                       |
| `baseAssetPrecision`  | float  | `0.001`   | Precision of the contract quantity (baseAsset).                                                                                                          |
| `quoteAsset`          | string | `USDT`    | Quote asset for the contract. Meaning the contract is quoted in that currency.                                                                           |
| `quoteAssetPrecision` | float  | `0.001`   | Precision of the contract price (quoteAsset).                                                                                                            |
| `inverse`             | bool   | `true`    | Whether the contract is inverse.                                                                                                                         |
| `index`               | string | `BTCUSDT` | Index symbol of the underlying asset. Index price can be accessed at the `index` endpoint. For instance, `BTC-PERP-USDT` uses `BTCUSDT` for index price. |
| `contractMultiplier`  | string | `true`    | The multiplier of contract.                                                                                                                              |
| `icebergAllowed`      | string | `false`   | Whether iceberg orders are allowed.                                                                                                                      |

For `filters` in `contracts` field:

| name          | type   | example           | description                           |
|---------------|--------|-------------------|---------------------------------------|
| `filterType`  | string | `PRICE_FILTER`    | Type of the filter.                   |
| `minPrice`    | float  | `0.001`           | Minimum price allowed                 |
| `maxPrice`    | float  | `100000.00000000` | Maximum price allowed                 |
| `tickSize`    | float  | `0.001`           | Precision of the contract price.      |
| `minQty`      | float  | `0.01`            | Minimum quantity allowed              |
| `maxQty`      | float  | `100000.00000000` | Maximum quantity allowed              |
| `stepSize`    | float  | `0.001`           | Precision of the contract quantity    |
| `minNotional` | float  | `1`               | Minimum trade size (quantity * price) |

In the `riskLimits` field:

| name            | type  | example | description                                                    |
|-----------------|-------|---------|----------------------------------------------------------------|
| `quantity`      | float | `100`   | Positions below this amount follows the following requirement. |
| `initialMargin` | float | `0.1`   | Initial margin rate requirement.                               |
| `maintMargin`   | float | `0.03`  | Minimum maintenance margin rate requirement.                   |

**Example:**

```json
{
  "timezone":"UTC",
  "serverTime":"1570701444309",
  "brokerFilters":[],
  "rateLimits":[
    {
      "rateLimitType":"ORDERS",
      "interval":"SECOND",
      "limit":20
    },
    {"rateLimitType":"ORDERS",
    "interval":"DAY",
    "limit":350000
  },{
    "rateLimitType":"REQUEST_WEIGHT",
    "interval":"MINUTE",
    "limit":1500
  }],
  "contracts":[
    {
      "filters":[
        {"minPrice":"0.01",
        "maxPrice":"100000.00000000",
        "tickSize":"0.01",
        "filterType":"PRICE_FILTER"
      },
      {
        "minQty":"1",
        "maxQty":"100000.00000000",
        "stepSize":"1",
        "filterType":"LOT_SIZE"
      },{
        "minNotional":"0.000001",
        "filterType":"MIN_NOTIONAL"
      }],
      "exchangeId":"301",
      "symbol":"BTC-PERP-USDT",
      "symbolName":"BTC-PERP-USDT",
      "status":"TRADING",
      "baseAsset":"BTC-PERP-USDT",
      "baseAssetPrecision":"1",
      "quoteAsset":"USDT",
      "quoteAssetPrecision":"0.01",
      "icebergAllowed":false,
      "inverse":true,
      "index":"BTCUSDT",
      "marginToken":"TBTC",
      "marginPrecision":"0.00000001",
      "contractMultiplier":"1.0",
      "riskLimits":[
        {
          "riskLimitId":"200000001",
          "quantity":"1000000.0",
          "initialMargin":"0.01",
          "maintMargin":"0.005"
        },
        {
          "riskLimitId":"200000002",
          "quantity":"2000000.0",
          "initialMargin":"0.02",
          "maintMargin":"0.01"
        },
        {
          "riskLimitId":"200000003",
          "quantity":"3000000.0",
          "initialMargin":"0.03",
          "maintMargin":"0.015"
        },
        {
          "riskLimitId":"200000004",
          "quantity":"4000000.0",
          "initialMargin":"0.04",
          "maintMargin":"0.02"
        }
      ]
    }
  ]
}
```

#### Contracts

```shell
GET /openapi/v1/contracts
```

Return all information regarding contracts

**Parameters:**

None

**Response:**

```json
[
  {
    "symbol":"BTC-PERP-USDT",
    "symbolName":"BTC-PERP-USDT",
    "baseToken":"BTC-PERP-USDT",
    "quoteToken":"TBTC",
    "lastPrice":"11682.8",
    "baseVolume":"13621303",
    "quoteVolume":"1094.487444013737088362",
    "bid":"11682",
    "ask":"11683.3",
    "high":"11730.1",
    "low":"11567.1",
    "productType":"futures",
    "openInterest":"23797354",
    "indexPrice":"11679.5433333333333333",
    "index":"BTCUSDT",
    "indexBaseToken":"USD",
    "startTs":"1598789083",
    "endTs":"1598875483",
    "fundingRate":"0.000038494841395324",
    "nextFundingRate":"0.000088260431150408",
    "nextFundingRateTs":1598904000
  },
  {
    "symbol":"BTC-USD",
    "symbolName":"BTC-USD",
    "baseToken":"BTC-USD",
    "quoteToken":"BTC",
    "lastPrice":"11682.2",
    "baseVolume":"27337579",
    "quoteVolume":"2348.305049590008405635",
    "bid":"11681.4",
    "ask":"11683.7",
    "high":"11730.6",
    "low":"11567.4",
    "productType":"futures",
    "openInterest":"3289364",
    "indexPrice":"11679.5433333333333333",
    "index":"BTCUSDT",
    "indexBaseToken":"USD",
    "startTs":"1598789092",
    "endTs":"1598875492",
    "fundingRate":"0.000084351317877269",
    "nextFundingRate":"0.000084285978912247",
    "nextFundingRateTs":1598904000
  }
]

```


--- 

### Market

Security Type: **None**

Endpoints under **Market** section can be accessed freely without requiring any API-key or signatures.

#### Index

```shell
GET /openapi/quote/v1/contract/index
```

Underlying asset index price.

**Weight:**
0

**Parameters:**

| name   | type   | required | description                                                                                         |
|--------|--------|----------|-----------------------------------------------------------------------------------------------------|
| symbol | string | `NO`     | Underlying index symbol. If this parameter is not sent, all symbols will be returned. E.g. `BTCUSD` |

**Response:**

| name    | type  | example   | description                                                                                                                                                                     |
|---------|-------|-----------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `index` | float | `28243.0` | The index price of the instrument.                                                                                                                                              |
| `EDP`   | float | `28258.5` | The EDP (estimated delivery price of the contract). The average price of the index in the last 1o minutes. This will be the price on which the contract is going to be settled. |

**Example:**

```json
{
  "index":{
    "BTCUSDT":"28243.0",
    "ETHUSDT":"1948.82",
    "BNBUSDT":"31.2658",
  },
  "edp":{
    "BTCUSDT":"28258.5",
    "ETHUSDT":"1956.853",
    "BNBUSDT":"31.4874",...
  }
}
```

#### Depth

```shell
GET /openapi/quote/v1/contract/depth
```
This endpoint retrieve contract market depth data. This endpoint updates every 3s.

**Parameters:**

| name     | type    | required | description                                                                  |
|----------|---------|----------|------------------------------------------------------------------------------|
| `symbol` | string  | `YES`    | The contract name for which to retrieve the order book. E.g. `BTC-PERP-USDT` |
| `limit`  | integer | `NO`     | The number of entries to return for bids and asks. Default 100; Max 100.     |

**Response:**

| name   | type | example         | description                                                     |
|--------|------|-----------------|-----------------------------------------------------------------|
| `time` | long | `1550829103981` | Current timestamp (ms)                                          |
| `bids` | list | (see below)     | List of all bids, best bids first. See below for entry details. |
| `asks` | list | (see below)     | List of all asks, best asks first. See below for entry details. |

The fields `bids` and `asks` are lists of orderbook price level entries, sorted from best to worst.

| name | type  | example  | description                                       |
|------|-------|----------|---------------------------------------------------|
| `''` | float | `123.10` | price level                                       |
| `''` | float | `300`    | The total quantity of orders for this price level |

**Example:**

```json
{
  "time": 1555049455783,
  "bids": [
     ["78.82", "0.526"], //[Price, Quantity]
     ["77.24", "1.22"],
     ["76.65", "1.043"],
     ["76.58", "1.34"],
     ["75.67", "1.52"],
     ["75.12", "0.635"],
     ["75.02", "0.72"],
     ["75.01", "0.672"],
     ["73.73", "1.282"],
     ["73.58", "1.116"],
     ["73.45", "0.471"],
     ["73.44", "0.483"],
     ["72.32", "0.383"],
     ["72.26", "1.283"],
     ["72.11", "0.703"],
     ["70.61", "0.454"]
  ],
  "asks": [
     ["122.96", "0.381"], //[Price, Quantity]
     ["144.46", "1"],
     ["155.55", "0.065"],
     ["160.16", "0.052"],
     ["200.00", "0.775"],
     ["249.00", "0.17"],
     ["250.00", "1"],
     ["300.00", "1"],
     ["400.00", "1"],
     ["499.00", "1"]
  ]
}
```

#### Merged depth

```shell
GET /openapi/quote/v1/contract/depth/merged
```

This endpoint retrieve contract market depth data. This endpoint updates every 0.5s.

**Parameters:**

| name     | type    | required | description                                                                  |
|----------|---------|----------|------------------------------------------------------------------------------|
| `symbol` | string  | `YES`    | The contract name for which to retrieve the order book. E.g. `BTC-PERP-USDT` |
| `limit`  | integer | `NO`     | The number of entries to return for bids and asks. Default 100; Max 100.     |

**Response:**

| name   | type | example         | description                                                     |
|--------|------|-----------------|-----------------------------------------------------------------|
| `time` | long | `1550829103981` | Current timestamp (ms)                                          |
| `bids` | list | (see below)     | List of all bids, best bids first. See below for entry details. |
| `asks` | list | (see below)     | List of all asks, best asks first. See below for entry details. |

The fields `bids` and `asks` are lists of orderbook price level entries, sorted from best to worst.

| name | type  | example  | description                                       |
|------|-------|----------|---------------------------------------------------|
| `''` | float | `123.10` | price level                                       |
| `''` | float | `300`    | The total quantity of orders for this price level |

**Example:**

```json
{
  "time": 1555049455783,
  "bids": [
     ["78.82", "0.526"], //[Price, Quantity]
     ["77.24", "1.22"],
     ["76.65", "1.043"],
     ["76.58", "1.34"],
     ["75.67", "1.52"],
     ["75.12", "0.635"],
     ["75.02", "0.72"],
     ["75.01", "0.672"],
     ["73.73", "1.282"],
     ["73.58", "1.116"],
     ["73.45", "0.471"],
     ["73.44", "0.483"],
     ["72.32", "0.383"],
     ["72.26", "1.283"],
     ["72.11", "0.703"],
     ["70.61", "0.454"]],
  "asks": [
     ["122.96", "0.381"], //[Price, Quantity]
     ["144.46", "1"],
     ["155.55", "0.065"],
     ["160.16", "0.052"],
     ["200", "0.775"],
     ["249", "0.17"],
     ["250", "1"],
     ["300", "1"],
     ["400", "1"],
     ["499", "1"]]
   }
```

#### Recent Trades List

```shell
GET /openapi/quote/v1/contract/trades
```

Get the latest trades that have occurred for a specific contract.

**Parameters:**

| Parameter | type    | required | description                                      |
|-----------|---------|----------|--------------------------------------------------|
| `symbol`  | string  | YES      | Symbol Name. E.g. `BTC-PERP-USDT`                |
| `limit`   | integer | NO       | Number of trades returned. Default 500; Max 1000 |

**Response:**

| name           | type   | example         | description                                                                                                            |
|----------------|--------|-----------------|------------------------------------------------------------------------------------------------------------------------|
| `price`        | float  | `0.055`         | The price of the trade                                                                                                 |
| `time`         | long   | `1537797044116` | Current timestamp (ms)                                                                                                 |
| `qty`          | float  | `5`             | The quantity traded                                                                                                    |
| `isBuyerMaker` | string | `true`          | Maker or taker of the trade. `true` = Order is a buy order when created, `false` = Order is a sell order when created  |

**Example:**
```json
[
  {
    "price": "4.00000100",
    "qty": "12.00000000",
    "time": 1499865549590,
    "isBuyerMaker": true
  },...
]
```

#### Klines

```shell
GET /openapi/quote/v1/klines
```

Retrieves the kline information (open, high, trade volume, etc.) for a specific contract.

**Parameters:**

| Parameter  | type    | required | description                                                                               |
|------------|---------|----------|-------------------------------------------------------------------------------------------|
| `symbol`   | string  | `YES`    | Name of the contract                                                                      |
| `interval` | string  | `YES`    | Interval of the kline. Possible values include: `1m`,`5m`,`15m`,`30m`,`1h`,`1d`,`1w`,`1M` |
| `endTime`  | integer | `NO`     | timestamp of the last datapoint                                                           |
| `limit`    | integer | `NO`     | Number of entries returned. Default 500; Max 1000                                         |

**Response:**

```json
[
  [
    1538728740000, //'opentime'
    "36.000000000000000000", //'open'
    "36.000000000000000000", //'high'
    "36.000000000000000000", //'low':
    "36.000000000000000000", //'close'
    "148976.11427815",  // Volume
    1499644799999,      // Close time
    "2434.19055334",    // Quote asset volume
    308,                // Number of trades
    "1756.87402397",    // Taker buy base asset volume
    "28.46694368"       // Taker buy quote asset volume
  ],...
]
```

* If startTime and endTime are not sent, the most recent klines are returned.
* If the symbol is not sent, tickers for all symbols will be returned in an array.
* `base asset` means the quantity is expressed as the amount of contracts that were received by the buyer.
* `quote asset` means the amount of tokens paid to acquire the contracts.

#### 24hrs ticker price change statistics

```shell
GET /openapi/quote/v1/contract/ticker/24hr
```

24hrs ticker price change statistics

**Parameters:**

| Parameter | type   | required | description                   |
|-----------|--------|----------|-------------------------------|
| `symbol`  | string | `NO`     | Symbol Name.  E.g. `BTC-USD`  |

**Response:**

| name           | type   | example         | description     |
|----------------|--------|-----------------|-----------------|
| `time`         | long   | `1538728740000` | Open Time       |
| `symbol`       | string | `BTC-USD`       | Contract Name   |
| `bestBidPrice` | float  | `4.000002000`   | Best Bid Price  |
| `bestAskPrice` | float  | `4.000002000`   | Best Ask Price  |
| `lastPrice`    | float  | `4.000002000`   | Last Price      |
| `openPrice`    | float  | `99.0000000`    | Open Price      |
| `highPrice`    | float  | `10.0000000`    | High Price      |
| `lowPrice`     | float  | `0.10000000`    | Low Price       |
| `volume`       | float  | `8913.300000`   | Trade Volume    |
| `openInterest` | float  | `21324243142`   | Open Interets   |

**Example:**

```json
// Single ticker 
{
  "time":1579424880100,
  "symbol":"BTC-USD",
  "bestBidPrice":"9079.1",
  "bestAskPrice":"9079.5",
  "volume":"61684269",
  "quoteVolume":"553667621070",
  "openInterest":"213245355",
  "lastPrice":"9077.5",
  "highPrice":"9179.8",
  "lowPrice":"8840.8",
  "openPrice":"8861.7"
}
// Multiple ticker info when symbol is omiited
[
  {
    "time":1579422004248,
    "symbol":"BTC-USD-USDT",
    "volume":"102696523",
    "quoteVolume":"918742352316.6",
    "openInterest":"213245355",
    "lastPrice":"9092.9",
    "highPrice":"9180.2",
    "lowPrice":"8817.7",
    "openPrice":"8817.7"
  },...
]
```

* If the symbol is not sent, tickers for all symbols will be returned in an array.

---

### Trade

Security Type: **[USER_DATA/TRADE](02-general-information.md#endpoint-security-type)**

Endpoints under **Trade** require an **[API-key and a signature](02-general-information.md#signed-trade-and-user_data-endpoint-security)**.









#### New order V1 (Deprecated)

```shell
POST /openapi/contract/v1/order (Deprecated)
```

Places order for a contract. This API endpoint requires your request to be signed.
(deprecated)

**Weight:**
1

**Headers:**

| Name      | Type   | Mandatory | Description  |
|-----------|--------|-----------|--------------|
| X-ACE-KEY | STRING | YES       | Your API key |

**Query Parameters:**

| name            | type   | required | description                                                                                             |
|-----------------|--------|----------|---------------------------------------------------------------------------------------------------------|
| `clientOrderId` | string | `NO`     | A unique ID of the order (user defined)                                                                 |
| `symbol`        | string | `YES`    | Name of the contract.                                                                                   |
| `side`          | string | `YES`    | Direction of the order. Possible values include `BUY_OPEN`, `SELL_OPEN`, `BUY_CLOSE`, and `SELL_CLOSE`. |
| `orderType`     | string | `YES`    | The order type, possible types: `LIMIT`, `STOP`                                                         |
| `quantity`      | float  | `YES`    | The number of contracts trading. **For `MARKET` buy orders, quantity is the equivalent of amount.**     |
| `leverage`      | float  | `YES`    | Leverage of the order (**NOT REQUIRED** for "\*\_CLOSE" orders)                                         |
| `price`         | float  | `NO`     | Price of the order. **REQUIRED** for (`LIMIT` & `INPUT`) orders                                         |
| `priceType`     | string | `NO`     | The price type, possible types include: `INPUT`, `OPPONENT`, `QUEUE`, `OVER`, and `MARKET`.             |
| `triggerPrice`  | float  | `NO`     | The price at which the trigger order will be executed. **REQUIRED** for `STOP` orders.                  |
| `timeInForce`   | string | `NO`     | Time in force for `LIMIT` orders. Possible values include `GTC`,`FOK`,`IOC`,`LIMIT_MAKER`.              |

**Body Parameters:**

| Name       | Type   | Mandatory | Description                                |
|------------|--------|-----------|--------------------------------------------|
| signature  | STRING | YES       | Authentication is needed for this endpoint |
| timestamp  | LONG   | YES       | Current unix timestamp(ms)                 |
| recvWindow | LONG   | NO        | RecvWindow for this request.               |

**Response:**

| Name            | type    | example              | description                                                                                                                |
|-----------------|---------|----------------------|----------------------------------------------------------------------------------------------------------------------------|
| `time`          | long    | `1570759718825`      | Timestamp when the order is created.                                                                                       |
| `updateTime`    | long    | `1551062936784`      | Last time this order was updated                                                                                           |
| `orderId`       | integer | `469961015902208000` | ID of the order.                                                                                                           |
| `clientOrderId` | string  | `213443`             | A unique ID of the order.                                                                                                  |
| `symbol`        | string  | `BTC-PERP-USDT`      | Name of the contract.                                                                                                      |
| `price`         | float   | `8200`               | Price of the order.                                                                                                        |
| `leverage`      | float   | `4`                  | Leverage of the order.                                                                                                     |
| `origQty`       | float   | `1.01`               | Quantity ordered                                                                                                           |
| `executedQty`   | float   | `1.01`               | Quantity of orders that has been executed                                                                                  |
| `avgPrice`      | float   | `4754.24`            | Average price of filled orders.                                                                                            |
| `marginLocked`  | float   | `200`                | Amount of margin locked for this order. This includes the actually margin needed plus fees to open and close the position. |
| `orderType`     | string  | `LIMIT`              | The order type, possible types: `LIMIT`, `STOP`                                                                            |
| `priceType`     | string  | `INPUT`              | The price type. Possible values include `INPUT`, `OPPONENT`, `QUEUE`, `OVER`, and `MARKET`.                                |
| `side`          | string  | `BUY`                | Direction of the order. Possible values include `BUY_OPEN`, `SELL_OPEN`, `BUY_CLOSE`, and `SELL_CLOSE`.                    |
| `status`        | string  | `NEW`                | The state of the order.Possible values include `NEW`, `PARTIALLY_FILLED`, `FILLED`, `CANCELED`, and `REJECTED`.            |
| `timeInForce`   | string  | `GTC`                | Time in force. Possible values include `GTC`,`FOK`,`IOC`, and `LIMIT_MAKER`                                                |
| `fees`          |         |                      | Fees incurred for this order.                                                                                              |

**Example:**

```json
{
  "time": "1570759718825",
  "updateTime": "0",
  "orderId": "469961015902208000",
  "clientOrderId": "6423344174",
  "symbol": "BTC-PERP-USDT",
  "price": "8200",
  "leverage": "12.08",
  "origQty": "5",
  "executedQty": "0",
  "avgPrice": "0",
  "marginLocked": "0.00005047",
  "orderType": "LIMIT",
  "side": "BUY_OPEN",
  "fees": [],
  "timeInForce": "GTC",
  "status": "NEW",
  "priceType": "INPUT"
}
```

**Request Weight:**
1

**NOTE**

For **Market Orders**, you need to set `orderType` as **`LIMIT`** **AND** `priceType` as **`MARKET`**.

You can get contracts' price, quantity precision configuration data in the `brokerInfo` endpoint.

Note: if your balance does not meet the margin requirement (which is the minimum margin requirement + open position fee + close position fee), "*insufficient balance*" error message will be returned.

For detailed information regarding various *price types* and *order types*. Please refer to the explanation section in the end.

(This API is Deprecated. Please use v2 order instead.)

.



#### New order V2 (recommended)

```shell
POST /openapi/contract/v2/order
```

Places order for a contract. This API endpoint requires your request to be signed.
Supports hedge mode and one-way mode.

**Weight:**
1

**Headers:**

| Name       | Type     | Mandatory | Description  |
|------------|----------|-----------|--------------|
| X-ACE-KEY  | STRING   | YES       | Your API key |

**Query Parameters:**

| name            | type    | required | description                                                                                                                                  |
|-----------------|---------|----------|----------------------------------------------------------------------------------------------------------------------------------------------|
| `clientOrderId` | string  | `NO`     | A unique ID of the order (user defined)                                                                                                      |
| `symbol`        | string  | `YES`    | Name of the contract.                                                                                                                        |
| `side`          | string  | `YES`    | Direction of the order. Possible values include `BUY_OPEN`, `SELL_OPEN`, `BUY_CLOSE`, `SELL_CLOSE`(HedgeMode) and `BUY`, `SELL`(OnewayMode). |
| `orderType`     | string  | `YES`    | The order type, possible types: `LIMIT`, `STOP`                                                                                              |
| `quantity`      | float   | `YES`    | The number of contracts trading. **For `MARKET` buy orders, quantity is the equivalent of amount.**                                          |
| `price`         | float   | `YES`    | Price of the order. **REQUIRED** for (`LIMIT` & `INPUT`) orders                                                                              |
| `priceType`     | string  | `YES`    | The price type, possible types include: `INPUT`, `MARKET`.                                                                                   |
| `triggerPrice`  | float   | `NO`     | The price at which the trigger order will be executed. **REQUIRED** for `STOP` orders.                                                       |
| `timeInForce`   | string  | `NO`     | Time in force for `LIMIT` orders. Possible values include `GTC`,`FOK`,`IOC` only.                                                            | 
| `isPostOnly`    | boolean | `YES`    | Order will be cancelled if executed immediately.                                                                                             | 
| `isReduceOnly`  | boolean | `YES`    | Orders with this option are only used to reduce positions.                                                                                   | 

**Body Parameters:**

| Name       | Type   | Mandatory | Description                                |
|------------|--------|-----------|--------------------------------------------|
| signature  | STRING | YES       | Authentication is needed for this endpoint |
| timestamp  | LONG   | YES       | Current unix timestamp(ms)                 |
| recvWindow | LONG   | NO        | RecvWindow for this request.               |

**Response:**

| Name            | type     | example              | description                                                                                                                                  |
|-----------------|----------|----------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| `time`          | long     | `1570759718825`      | Timestamp when the order is created.                                                                                                         |
| `updateTime`    | long     | `1551062936784`      | Last time this order was updated                                                                                                             |
| `orderId`       | integer  | `469961015902208000` | ID of the order.                                                                                                                             |
| `clientOrderId` | string   | `213443`             | A unique ID of the order.                                                                                                                    |
| `symbol`        | string   | `BTC-PERP-USDT`      | Name of the contract.                                                                                                                        |
| `price`         | float    | `8200`               | Price of the order.                                                                                                                          |
| `leverage`      | float    | `4`                  | Leverage of the order.                                                                                                                       |
| `origQty`       | float    | `1000`               | Quantity ordered                                                                                                                             |
| `executedQty`   | float    | `777`                | Quantity of orders that has been executed                                                                                                    |
| `avgPrice`      | float    | `4754.24`            | Average price of filled orders.                                                                                                              |
| `marginLocked`  | float    | `200`                | Amount of margin locked for this order. This includes the actually margin needed plus fees to open and close the position.                   |
| `orderType`     | string   | `LIMIT`              | The order type, possible types: `LIMIT`, `STOP`                                                                                              |
| `side`          | string   | `BUY`                | Direction of the order. Possible values include `BUY_OPEN`, `SELL_OPEN`, `BUY_CLOSE`, `SELL_CLOSE`(HedgeMode) and `BUY`, `SELL`(OnewayMode). |
| `fees`          |          |                      | Fees incurred for this order.                                                                                                                |
| `timeInForce`   | string   | `GTC`                | Time in force. Possible values include `GTC`,`FOK`,`IOC`                                                                                     |
| `status`        | string   | `NEW`                | The state of the order.Possible values include `NEW`, `PARTIALLY_FILLED`, `FILLED`, `CANCELED`, and `REJECTED`.                              |
| `priceType`     | string   | `INPUT`              | The price type. Possible values include `INPUT`, `MARKET`.                                                                                   |
| `isPostOnly`    | boolean  | `true`               | Order will be cancelled if executed immediately.                                                                                             |
| `isReduceOnly`  | boolean  | `false`              | Orders with this option are only used to reduce positions.                                                                                   |

**Example:**

```json
{
  "time": "1694498706716",
  "updateTime": "1694498706716",
  "orderId": "1507958879526339328",
  "clientOrderId": "4533207151694498705868",
  "symbol": "BTC-USDT",
  "price": "22220.5",
  "leverage": "100",
  "origQty": "19",
  "executedQty": "0",
  "avgPrice": "0",
  "marginLocked": "0.4221",
  "orderType": "LIMIT",
  "side": "BUY_OPEN",
  "fees": [],
  "timeInForce": "GTC",
  "status": "NEW",
  "priceType": "INPUT",
  "isPostOnly": false,
  "isReduceOnly": false
}
```

**Request Weight:**
1

**NOTE**

For **Market Orders**, you need to set `orderType` as **`LIMIT`** **AND** `priceType` as **`MARKET`**.

You can get contracts' price, quantity precision configuration data in the `brokerInfo` endpoint.

Note: if your balance does not meet the margin requirement (which is the minimum margin requirement + open position fee + close position fee), "*insufficient balance*" error message will be returned.

For detailed information regarding various *price types* and *order types*. Please refer to the explanation section in the end.


#### Query Trade Setting

```shell
GET /openapi/contract/v2/getTradeSetting
```

Returns the trade settings of the specified user for each trading pair.

**Headers:**

| Name      | Type   | Mandatory | Description  |
|-----------|--------|-----------|--------------|
| X-ACE-KEY | STRING | YES       | Your API key |

**Query Parameters:**

| name            | type   | required | description |
|-----------------|--------|----------|-------------|
| `symbolId `     | string | `YES`    |             |

**Body Parameters:**

| Name       | Type   | Mandatory | Description                                |
|------------|--------|-----------|--------------------------------------------|
| signature  | STRING | YES       | Authentication is needed for this endpoint |
| timestamp  | LONG   | YES       | Current unix timestamp(ms)                 |
| recvWindow | LONG   | NO        | RecvWindow for this request.               |

**Response:**

| Name            | type   | example          | description                                                               |                                                       
|-----------------|--------|------------------|---------------------------------------------------------------------------|
| `symbolId`      | string | `BTC-PERP-USDT`  | Name of the contracts.                                                    |                                                      
| `positionMode`  | int    | `0`              | Current PositionMode Value. Hedge Mode = 0, Oneway Mode = 1               |                                                                             
| `marginMode`    | int    | `1`              | Current MarginMode Value. Isolated Margin Mode = 0, Cross Margin Mode = 1 |                                                                      
| `leverage`      | float  | `10.00`          | Current Trading pair Leverage Value ( 1x ~ 100x )                         |                                                               

**Example:**

```json
{
  "symbolId": "BTC-PERP-USDT",
  "positionMode": 0,
  "marginMode": 1,
  "leverage": "10.00"
}
```



#### adjust position mode

```shell
POST /openapi/contract/v2/adjustPositionMode
```

adjust position mode of trading pair

**Headers:**

| Name      | Type   | Mandatory | Description  |
|-----------|--------|-----------|--------------|
| X-ACE-KEY | STRING | YES       | Your API key |

**Query Parameters:**

| name           | type    | required | description                                           |
|----------------|---------|----------|-------------------------------------------------------|
| `symbolId`     | string  | `YES`    |                                                       |
| `positionMode` | int     | `YES`    | PositionMode Value. Hedge Mode = 0, Oneway Mode = 1   |

**Body Parameters:**

| Name       | Type   | Mandatory | Description                                |
|------------|--------|-----------|--------------------------------------------|
| signature  | STRING | YES       | Authentication is needed for this endpoint |
| timestamp  | LONG   | YES       | Current unix timestamp(ms)                 |
| recvWindow | LONG   | NO        | RecvWindow for this request.               |

**Response:**

| Name            | type   | example          | description                                          |                                                       
|-----------------|--------|------------------|------------------------------------------------------|
| `symbolId`      | string | `BTC-PERP-USDT`  | Name of the contracts.                               |                                                      
| `positionMode`  | int    | `0`              | PositionMode Value. Hedge Mode = 0, Oneway Mode = 1  |                                                              

**Example:**

```json
{
  "symbolId": "BTC-PERP-USDT",
  "positionMode": 0
}
```


#### adjust margin mode

```shell
POST /openapi/contract/v2/adjustMarginMode
```

adjust margin mode of trading pair

**Headers:**

| Name       | Type   | Mandatory | Description  |
|------------|--------|-----------|--------------|
| X-ACE-KEY  | STRING | YES       | Your API key |

**Query Parameters:**

| name           | type    | required | description                                                         |
|----------------|---------|----------|---------------------------------------------------------------------|
| `symbolId`     | string  | `YES`    |                                                                     |
| `marginMode`   | int     | `YES`    | MarginMode Value. Isolated Margin Mode = 0, Cross Margin Mode = 1   |

**Body Parameters:**

| Name       | Type   | Mandatory | Description                                  |
|------------|--------|-----------|----------------------------------------------|
| signature  | STRING | YES       | Authentication is needed for this endpoint   |
| timestamp  | LONG   | YES       | Current unix timestamp(ms)                   |
| recvWindow | LONG   | NO        | RecvWindow for this request.                 |

**Response:**

| Name            | type   | example          | description                                                         |                                                       
|-----------------|--------|------------------|---------------------------------------------------------------------|
| `symbolId`      | string | `BTC-PERP-USDT`  | Name of the contracts.                                              |                                                      
| `marginMode`    | int    | `0`              | MarginMode Value. Isolated Margin Mode = 0, Cross Margin Mode = 1   |                                                              

**Example:**

```json
{
  "symbolId": "BTC-PERP-USDT",
  "marginMode": 0
}
```


#### adjust leverage

```shell
POST /openapi/contract/v2/adjustLeverageMode
```

adjust leverage value

**Headers:**

| Name      | Type   | Mandatory | Description  |
|-----------|--------|-----------|--------------|
| X-ACE-KEY | STRING | YES       | Your API key |

**Query Parameters:**

| name           | type    | required  | description             |
|----------------|---------|-----------|-------------------------|
| `symbolId`     | string  | `YES`     | Name of the contracts.  |
| `leverage`     | int     | `YES`     | Leverage Value          |

**Body Parameters:**

| Name        | Type   | Mandatory | Description                                  |
|-------------|--------|-----------|----------------------------------------------|
| signature   | STRING | YES       | Authentication is needed for this endpoint   |
| timestamp   | LONG   | YES       | Current unix timestamp(ms)                   |
| recvWindow  | LONG   | NO        | RecvWindow for this request.                 |

**Response:**

| Name       | type   | example         | description              |                                                       
|------------|--------|-----------------|--------------------------|
| `symbolId` | string | `BTC-PERP-USDT` | Name of the contracts.   |                                                      
| `leverage` | float  | `10.00`         | Leverage Value           |                                                              

**Example:**

```json
{
  "symbolId": "BTC-PERP-USDT",
  "leverage": "10.00"
}
```


#### Query order

```shell
GET /openapi/contract/v1/getOrder
```

Get details on a specific order, regardless of order state.

**Headers:**

| Name      | Type   | Mandatory | Description  |
|-----------|--------|-----------|--------------|
| X-ACE-KEY | STRING | YES       | Your API key |

**Query Parameters:**

| name            | type   | required | description                                                                    |
|-----------------|--------|----------|--------------------------------------------------------------------------------|
| `orderType`     | string | `YES`    | The order type, possible types: `LIMIT`, `STOP`                                |
| `orderId`       | string | `NO`     | Order Id. E.g. `507904211109878016`                                            |
| `clientOrderId` | string | `NO`     | A unique ID of the order (user defined) E.g. `1670093910_custom_unique_string` | 

**Body Parameters:**

| Name       | Type   | Mandatory | Description                                |
|------------|--------|-----------|--------------------------------------------|
| signature  | STRING | YES       | Authentication is needed for this endpoint |
| timestamp  | LONG   | YES       | Current unix timestamp(ms)                 |
| recvWindow | LONG   | NO        | RecvWindow for this request.               |

**Response:**

| Name            | type    | example         | description                                                                                                                |
|-----------------|---------|-----------------|----------------------------------------------------------------------------------------------------------------------------|
| `time`          | long    | `1551062936784` | Timestamp when the order is created.                                                                                       |
| `updateTime`    | long    | `1551062936784` | Last time this order was updated                                                                                           |
| `orderId`       | integer | `891`           | ID of the order.                                                                                                           |
| `clientOrderId` | string  | `213443`        | A unique ID of the order.                                                                                                  |
| `symbol`        | string  | `BTC-PERP-USDT` | Name of the contracts.                                                                                                     |
| `price`         | float   | `4765.29`       | Price of the order.                                                                                                        |
| `leverage`      | float   | `4`             | Leverage of the order.                                                                                                     |
| `origQty`       | float   | `1.01`          | Quantity ordered                                                                                                           |
| `executedQty`   | float   | `1.01`          | Quantity of orders that has been executed                                                                                  |
| `avgPrice`      | float   | `4754.24`       | Average price of filled orders.                                                                                            |
| `marginLocked`  | float   | `200`           | Amount of margin locked for this order. This includes the actually margin needed plus fees to open and close the position. |
| `orderType`     | string  | `LIMIT`         | The order type, possible types: `LIMIT` and `STOP`.                                                                        |
| `priceType`     | string  | `INPUT`         | The price type. Possible values include `INPUT`, `OPPONENT`, `QUEUE`, `OVER`, and `MARKET`.                                |
| `side`          | string  | `BUY_OPEN`      | Direction of the order. Possible values include `BUY_OPEN`, `SELL_OPEN`, `BUY_CLOSE`, and `SELL_CLOSE`.                    |
| `status`        | string  | `NEW`           | The state of the order.Possible values include `NEW`, `PARTIALLY_FILLED`, `FILLED`, `CANCELED`, and `REJECTED`.            |
| `timeInForce`   | string  | `GTC`           | Time in force. Possible values include `GTC`,`FOK`,`IOC`, and `LIMIT_MAKER`.                                               |
| `fees`          |         |                 | Fees incurred for this order.                                                                                              |
| `isPostOnly`    | boolean | `true`          | Order will be cancelled if executed immediately.                                                                           |
| `isReduceOnly`  | boolean | `false`         | Orders with this option are only used to reduce positions.                                                                 |

**Example:**

```json
{
  "time": "1570760254539",
  "updateTime": "0",
  "orderId": "469965509788581888",
  "clientOrderId": "1570760253946",
  "symbol": "BTC-PERP-USDT",
  "price": "8502.34",
  "leverage": "20",
  "origQty": "222",
  "executedQty": "0",
  "avgPrice": "0",
  "marginLocked": "0.00130552",
  "orderType": "LIMIT",
  "side": "BUY_OPEN",
  "fees": [],
  "timeInForce": "GTC",
  "status": "NEW",
  "priceType": "INPUT",
  "isPostOnly": false,
  "isReduceOnly": false
}
```

**Request Weight:**
1

* **Either `orderId` or `clientOrderId` must be sent**


#### Cancel

```shell
DELETE /openapi/contract/v1/order/cancel
```

Cancels an order, specified by `orderId` or `clientOrderId`

**Headers:**

| Name      | Type   | Mandatory | Description  |
|-----------|--------|-----------|--------------|
| X-ACE-KEY | STRING | YES       | Your API key |

**Query Parameters:**

| name            | type   | required | description                                                   |
|-----------------|--------|----------|---------------------------------------------------------------|
| `orderType`     | string | `YES`    | The order type, possible types: `LIMIT`, `STOP`               |
| `orderId`       | string | `NO`     | Order Id. E.g. `507904211109878016`                           |
| `clientOrderId` | string | `NO`     | A unique ID of the order (user defined) E.g. `12094ahsihsiad` |

**Body Parameters:**

| Name       | Type   | Mandatory | Description                                |
|------------|--------|-----------|--------------------------------------------|
| signature  | STRING | YES       | Authentication is needed for this endpoint |
| timestamp  | LONG   | YES       | Current unix timestamp(ms)                 |
| recvWindow | LONG   | NO        | RecvWindow for this request.               |

**Response:**

| Name            | type    | example         | description                                                                                                                |
|-----------------|---------|-----------------|----------------------------------------------------------------------------------------------------------------------------|
| `time`          | long    | `1551062936784` | Timestamp when the order is created.                                                                                       |
| `updateTime`    | long    | `1551062936784` | Last time this order was updated                                                                                           |
| `orderId`       | integer | `891`           | ID of the order.                                                                                                           |
| `clientOrderId` | string  | `213443`        | A unique ID of the order.                                                                                                  |
| `symbol`        | string  | `BTC-PERP-USDT` | Name of the contract.                                                                                                      |
| `price`         | float   | `4765.29`       | Price of the order.                                                                                                        |
| `leverage`      | float   | `4`             | Leverage of the order.                                                                                                     |
| `origQty`       | float   | `1.01`          | Quantity ordered                                                                                                           |
| `executedQty`   | float   | `1.01`          | Quantity of orders that has been executed                                                                                  |
| `avgPrice`      | float   | `4754.24`       | Average price of filled orders.                                                                                            |
| `marginLocked`  | float   | `200`           | Amount of margin locked for this order. This includes the actually margin needed plus fees to open and close the position. |
| `orderType`     | string  | `LIMIT`         | The order type, possible types: `LIMIT` and `STOP`.                                                                        |
| `priceType`     | string  | `INPUT`         | The price type. Possible values include `INPUT`, `OPPONENT`, `QUEUE`, `OVER`, and `MARKET`.                                |
| `side`          | string  | `BUY_OPEN`      | Direction of the order. Possible values include `BUY_OPEN`, `BUY_CLOSE`, `SELL_OPEN` and `SELL_CLOSE`                      |
| `status`        | string  | `NEW`           | The state of the order.Possible values include `NEW`, `PARTIALLY_FILLED`, `FILLED`, `CANCELED`, and `REJECTED`.            |
| `timeInForce`   | string  | `GTC`           | Time in force. Possible values include `GTC`,`FOK`,`IOC`, and `LIMIT_MAKER`                                                |
| `fees`          |         |                 | Fees incurred for this order.                                                                                              |
| `isPostOnly`    | boolean | `true`          | Order will be cancelled if executed immediately.                                                                           |
| `isReduceOnly`  | boolean | `false`         | Orders with this option are only used to reduce positions.                                                                 |

In the `fees` field:

| Name       | type   | example | description                       |
|------------|--------|---------|-----------------------------------|
| `feeToken` | string | `USDT`  | Fee token kind.                   |
| `fee`      | float  | `0`     | Actual transaction fees occurred. |

**Request Weight:**
1

* Either `orderId` or `clientOrderId` must be sent

**Example:**

```json
{
  "time": "1570759718825",
  "updateTime": "1694662998125",
  "orderId": "1509336877311860000",
  "clientOrderId": "694662976799",
  "symbol": "BTC-PERP-USDT",
  "price": "22222",
  "leverage": "100",
  "origQty": "10000",
  "executedQty": "0",
  "avgPrice": "0",
  "marginLocked": "0",
  "orderType": "LIMIT",
  "side": "BUY_OPEN",
  "fees": [],
  "timeInForce": "GTC",
  "status": "CANCELED", //status will always be `CANCELED` for cancel request
  "priceType": "INPUT",
  "isPostOnly": false,
  "isReduceOnly": false
}
```

#### Batch cancel

```shell
DELETE /openapi/contract/v1/order/batchCancel
```

Cancel all **LIMIT** orders.

**Headers:**

| Name      | Type   | Mandatory | Description  |
|-----------|--------|-----------|--------------|
| X-ACE-KEY | STRING | YES       | Your API key |

**Query Parameters:**

| name   | type   | required | description                                              |
|--------|--------|----------|----------------------------------------------------------|
| symbol | string | `NO`     | The symbol ids of the cancel orders. E.g. BTC-PERP-USDT  |

**Body Parameters:**

| Name       | Type   | Mandatory | Description                                |
|------------|--------|-----------|--------------------------------------------|
| signature  | STRING | YES       | Authentication is needed for this endpoint |
| timestamp  | LONG   | YES       | Current unix timestamp(ms)                 |
| recvWindow | LONG   | NO        | RecvWindow for this request.               |

**Response:**

| Name        | type   | example         | description                                  |
|-------------|--------|-----------------|----------------------------------------------|
| `message`   | string | `success`       | The message response of the cancel request.  |
| `timestamp` | long   | `1541161088303` | The timestamp when the response is returned. |

**Example:**

```json
{
  "message": "success",
  "timestamp": 1541161088303
}
```

#### Current open orders

```shell
GET /openapi/contract/v1/openOrders
```

Get all open orders on a symbol. Careful when accessing this with no symbols.

**Headers:**

| Name      | Type   | Mandatory | Description  |
|-----------|--------|-----------|--------------|
| X-ACE-KEY | STRING | YES       | Your API key |

**Query Parameters:**

| name        | type    | required | description                                                                              |
|-------------|---------|----------|------------------------------------------------------------------------------------------|
| `orderType` | string  | `YES`    | The order type, possible types: `LIMIT` and `STOP`.                                      |
| `symbol`    | string  | `NO`     | Symbol to return open orders for. If not sent, orders of all contracts will be returned. |
| `orderId`   | integer | `NO`     | Order ID                                                                                 |
| `limit`     | integer | `NO`     | Number of entries to return. Default 20; Max 1000                                        |

**Body Parameters:**

| Name       | Type   | Mandatory | Description                                |
|------------|--------|-----------|--------------------------------------------|
| signature  | STRING | YES       | Authentication is needed for this endpoint |
| timestamp  | LONG   | YES       | Current unix timestamp(ms)                 |
| recvWindow | LONG   | NO        | RecvWindow for this request.               |


**Response:**

| Name            | type    | example         | description                                                                                                                |
|-----------------|---------|-----------------|----------------------------------------------------------------------------------------------------------------------------|
| `time`          | long    | `1551062936784` | Timestamp when the order is created.                                                                                       |
| `updateTime`    | long    | `1551062936784` | Last time this order was updated                                                                                           |
| `orderId`       | integer | `891`           | ID of the order.                                                                                                           |
| `clientOrderId` | string  | `213443`        | A unique ID of the order.                                                                                                  |
| `symbol`        | string  | `BTC-PERP-USDT` | Name of the contracts.                                                                                                     |
| `price`         | float   | `4765.29`       | Price of the order.                                                                                                        |
| `leverage`      | float   | `4`             | Leverage of the order.                                                                                                     |
| `origQty`       | float   | `1.01`          | Quantity ordered                                                                                                           |
| `executedQty`   | float   | `1.01`          | Quantity of orders that has been executed                                                                                  |
| `avgPrice`      | float   | `4754.24`       | Average price of filled orders.                                                                                            |
| `marginLocked`  | float   | `200`           | Amount of margin locked for this order. This includes the actually margin needed plus fees to open and close the position. |
| `orderType`     | string  | `LIMIT`         | The order type, possible types: `LIMIT` and `STOP`.                                                                        |
| `priceType`     | string  | `INPUT`         | The price type. Possible values include `INPUT`, `OPPONENT`, `QUEUE`, `OVER`, and `MARKET`.                                |
| `side`          | string  | `BUY_OPEN`      | Direction of the order. Possible values include `BUY_OPEN`, `SELL_OPEN`, `BUY_CLOSE`, and `SELL_CLOSE`.                    |
| `status`        | string  | `NEW`           | The state of the order.Possible values include `NEW`, `PARTIALLY_FILLED`, `FILLED`, `CANCELED`, and `REJECTED`.            |
| `timeInForce`   | string  | `GTC`           | Time in force. Possible values include `GTC`,`FOK`,`IOC`, and `LIMIT_MAKER`.                                               |
| `fees`          |         |                 | Fees incurred for this order.                                                                                              |
| `isPostOnly`    | boolean | `true`          | Order will be cancelled if executed immediately.                                                                           |
| `isReduceOnly`  | boolean | `false`         | Orders with this option are only used to reduce positions.                                                                 |

**Request Weight:**
1

* If `orderId` is set, it will get orders < that `orderId`. Otherwise most recent orders are returned.

**Example:**

```json
[
  {
    "time": "1570760254539",
    "updateTime": "0",
    "orderId": "469965509788581888",
    "clientOrderId": "1570760253946",
    "symbol": "BTC-PERP-USDT",
    "price": "8502.34",
    "leverage": "20",
    "origQty": "222",
    "executedQty": "0",
    "avgPrice": "0",
    "marginLocked": "0.00130552",
    "orderType": "LIMIT",
    "side": "BUY_OPEN",
    "fees": [],
    "timeInForce": "GTC",
    "status": "NEW",
    "priceType": "INPUT",
    "isPostOnly": false,
    "isReduceOnly": false
  }
]
```

#### History orders

```shell
GET /openapi/contract/v1/historyOrders
```

Retrieves history of orders that have been partially or fully filled or canceled. Careful when accessing this with no symbol.

**Headers:**

| Name      | Type   | Mandatory | Description     |
|-----------|--------|-----------|-----------------|
| X-ACE-KEY | STRING | YES       | Your API key    |

**Query Parameters:**

| name        | type    | required | description                                           |
|-------------|---------|----------|-------------------------------------------------------|
| `orderType` | string  | `YES`    | The order type, possible types: `LIMIT` and `STOP`.   |
| `symbol`    | string  | `NO`     | Symbol Name. E.g. `BTC-PERP-USDT`.                    |
| `orderId`   | integer | `NO`     | Order ID. E.g., `507904211109878016`.                 |
| `limit`     | integer | `NO`     | Number of entries to return. Default 500; Max 1000.   |


**Body Parameters:**

| Name         | Type   | Mandatory | Description                                 |
|--------------|--------|-----------|---------------------------------------------|
| `signature`  | STRING | YES       | Authentication is needed for this endpoint. |
| `timestamp`  | LONG   | YES       | Current unix timestamp (ms).                |
| `recvWindow` | LONG   | NO        | RecvWindow for this request.                |


**Response:**

| Name            | Type    | Example         | Description                                                                                                                |
|-----------------|---------|-----------------|----------------------------------------------------------------------------------------------------------------------------|
| `time`          | long    | 1551062936784   | Timestamp when the order is created.                                                                                       |
| `updateTime`    | long    | 1551062936784   | Last time this order was updated.                                                                                          |
| `orderId`       | integer | 891             | ID of the order.                                                                                                           |
| `clientOrderId` | string  | 213443          | A unique ID of the order.                                                                                                  |
| `symbol`        | string  | BTC-PERP-USDT   | Name of the contracts.                                                                                                     |
| `price`         | float   | 4765.29         | Price of the order.                                                                                                        |
| `leverage`      | float   | 4               | Leverage of the order.                                                                                                     |
| `origQty`       | float   | 1000            | Quantity ordered.                                                                                                          |
| `executedQty`   | float   | 777            | Quantity of orders that has been executed.                                                                                 |
| `avgPrice`      | float   | 4754.24         | Average price of filled orders.                                                                                            |
| `marginLocked`  | float   | 200             | Amount of margin locked for this order. This includes the actually margin needed plus fees to open and close the position. |
| `orderType`     | string  | LIMIT           | The order type, possible types: LIMIT and STOP.                                                                            |
| `priceType`     | string  | INPUT           | The price type. Possible values include INPUT, OPPONENT, QUEUE, OVER, and MARKET.                                          |
| `side`          | string  | BUY_OPEN        | Direction of the order. Possible values include BUY_OPEN, SELL_OPEN, BUY_CLOSE, and SELL_CLOSE.                            |
| `status`        | string  | NEW             | The state of the order. Possible values include NEW, PARTIALLY_FILLED, FILLED, CANCELED, and REJECTED.                     |
| `timeInForce`   | string  | GTC             | Time in force. Possible values include GTC, FOK, IOC, and LIMIT_MAKER.                                                     |
| `fees`          |         |                 | Fees incurred for this order.                                                                                              |
| `isPostOnly`    | boolean | `true`          | Order will be cancelled if executed immediately.                                                                           |
| `isReduceOnly`  | boolean | `false`         | Orders with this option are only used to reduce positions.                                                                 |

**Example:**

```json
[
  {
    "time": "1570759718825",
    "updateTime": "0",
    "orderId": "469961015902208000",
    "clientOrderId": "6423344174",
    "symbol": "BTC-PERP-USDT",
    "price": "8200",
    "leverage": "12.08",
    "origQty": "5",
    "executedQty": "0",
    "avgPrice": "0",
    "marginLocked": "0",
    "orderType": "LIMIT",
    "side": "BUY_OPEN",
    "fees": [],
    "timeInForce": "GTC",
    "status": "CANCELED",
    "priceType": "INPUT",
    "isPostOnly": false,
    "isReduceOnly": false
  },...
]
```

**Request Weight:**
5

* If `orderId` is set, it will get orders < that `orderId`. Otherwise most recent orders are returned.

#### Trades

```shell
GET /openapi/contract/v1/myTrades
```

Retrieve the trade history of the account.

**Headers:**

| Name      | Type   | Mandatory | Description     |
|-----------|--------|-----------|-----------------|
| X-ACE-KEY | STRING | YES       | Your API key    |


**Query Parameters:**

## Query Parameters

| name     | type    | required | description                                                                             |
|----------|---------|----------|-----------------------------------------------------------------------------------------|
| `symbol` | string  | `NO`     | Symbol Name. If not sent, trades for all symbols will be returned. E.g. `BTC-PERP-USDT` |
| `fromId` | integer | `NO`     | Trade Id to fetch from                                                                  |
| `toId`   | integer | `NO`     | Trade Id to fetch to                                                                    |
| `limit`  | integer | `NO`     | Number of entries to return. Default 20; Max 1000                                       |


**Body Parameters:**

| Name       | Type   | Mandatory | Description                                |
|------------|--------|-----------|--------------------------------------------|
| signature  | STRING | YES       | Authentication is needed for this endpoint |
| timestamp  | LONG   | YES       | Current unix timestamp(ms)                 |
| recvWindow | LONG   | NO        | RecvWindow for this request.               |

**Response:**

## Response

| Name           | Type    | Example         | Description                                                                                                                                 |
|----------------|---------|-----------------|---------------------------------------------------------------------------------------------------------------------------------------------|
| `time`         | long    | `1551062936784` | Timestamp when the order is created.                                                                                                        |
| `tradeId`      | long    | `49366`         | The ID for the trade                                                                                                                        |
| `orderId`      | integer | `891`           | ID of the order.                                                                                                                            |
| `matchOrderId` | long    | `630491432`     | ID of the match order.                                                                                                                      |
| `symbolId`     | string  | `BTC-PERP-USDT` | Name of the contract.                                                                                                                       |
| `price`        | float   | `4765.29`       | Price of the trade.                                                                                                                         |
| `quantity`     | float   | `1.01`          | Quantity of the trade.                                                                                                                      |
| `feeTokenId`   | string  | `USDT`          | Fee token name.                                                                                                                             |
| `fee`          |         |                 | Fee of the trade.                                                                                                                           |
| `side`         | string  | `BUY`           | Direction of the order. Possible values include `BUY_OPEN`, `SELL_OPEN`, `BUY_CLOSE`, `SELL_CLOSE`(HedgeMode) and `BUY`, `SELL`(OnewayMode).|
| `orderType`    | string  | `LIMIT`         | The order type, possible types: LIMIT, MARKET                                                                                               |
| `pnl`          | float   | `100.1`         | Profit and loss                                                                                                                             |


**Example:**

```json
[
  {
    "time": "1570760582848",
    "tradeId": "469968263995080704",
    "orderId": "469968263793737728",
    "matchOrderId": 436002617267469062,
    "accountId": "456552319339779840",
    "symbolId": "BTC-PERP-USDT",
    "price": "8531.17",
    "quantity": "100",
    "feeTokenId": "TBTC",
    "fee": "0.00000586",
    "type": "LIMIT",
    "side": "BUY_OPEN",
    "pnl": "100.1"
  },...
]
```

**Request Weight:**
1

* If only `fromId` is set，it will get orders < that `fromId` in descending order.

* If only `toId` is set, it will get orders > that `toId` in ascending order.

* If `fromId` is set and `toId` is set, it will get orders < that `fromId` and > that `toId` in descending order.

* If `fromId` is not set and `toId` it not set, most recent order are returned in descending order.

#### Positions

```shell
GET /openapi/contract/v1/positions
```

Retrieves current positions.

**Headers:**

| Name      | Type   | Mandatory | Description  |
|-----------|--------|-----------|--------------|
| X-ACE-KEY | STRING | YES       | Your API key |

**Query Parameters:**

| name     | type   | required | description                                                                                             |
|----------|--------|----------|---------------------------------------------------------------------------------------------------------|
| `symbol` | string | `NO`     | Symbol Name. If not sent, trades for all symbols will be returned. E.g. `BTC-PERP-USDT`                 |
| `side`   | string | `NO`     | `LONG` or `SHORT`. Direction of the position. If not sent, positions for both sides will be returned.   |

**Body Parameters:**

| Name       | Type   | Mandatory | Description                                |
|------------|--------|-----------|--------------------------------------------|
| signature  | STRING | YES       | Authentication is needed for this endpoint |
| timestamp  | LONG   | YES       | Current unix timestamp(ms)                 |
| recvWindow | LONG   | NO        | RecvWindow for this request.               |

**Response:**

| Name            | type   | example         | description                                             |
|-----------------|--------|-----------------|---------------------------------------------------------|
| `symbol`        | string | `BTC-PERP-USDT` | Name of the contract.                                   |
| `side`          | string | `LONG`          | Position side.                                          |
| `avgPrice`      | float  | `100`           | Average price for opening the position.                 |
| `position`      | float  | `20`            | Amount of contracts opened                              |
| `available`     | float  | `15`            | Amount of contracts available to close.                 |
| `leverage`      | float  | `5`             | Leverage of the position                                |
| `lastPrice`     | float  | `100`           | Last trade price of the symbol.                         |
| `positionValue` | float  | `2000`          | Current position value.                                 |
| `flp`           | float  | `80`            | Forced liquidation price.                               |
| `margin`        | float  | `20`            | Margin for this position.                               |
| `marginRate`    | float  | `0.2`           | Margin rate for current position.                       |
| `unrealizedPnL` | float  | `0.0`           | Unrealized profit and loss for current position held.   |
| `profitRate`    | float  | `0.0000333`     | Rate of return for the position.                        |
| `realizedPnL`   | float  | `6.8`           | Cumulative realized profit and loss for this `symbol`.  |

**Example:**
```json
[
  {
    "symbol": "BTC-PERP-USDT",
    "side": "LONG",
    "avgPrice": "8183.11",
    "position": "1100",
    "available": "1100",
    "leverage": "6",
    "lastPrice": "8572.53",
    "positionValue": "0.12833346",
    "flp": "7523.65",
    "margin": "0.01251335",
    "marginRate": "0.14",
    "unrealizedPnL": "0.00608975",
    "profitRate": "0.0000333",
    "realizedPnL": "-0.00006721"
  },...
]
```

**Request Weight:**
1

#### Position Risk Limits

```shell
GET /openapi/contract/v1/positionRiskLimits
```

Retrieves currently applied risk limit levels for the positions of the symbols sent

**Headers:**

| Name      | Type   | Mandatory | Description  |
|-----------|--------|-----------|--------------|
| X-ACE-KEY | STRING | YES       | Your API key |

**Query Parameters:**

| name         | type   | required | description                                   |
|--------------|--------|----------|-----------------------------------------------|
| `symbol_ids` | string | `YES`    | symbol ids E.g. `BTC-PERP-USDT,ETH-PERP-USDT` | 

**Body Parameters:**

| Name       | Type   | Mandatory | Description                                  |
|------------|--------|-----------|----------------------------------------------|
| signature  | STRING | YES       | Authentication is needed for this endpoint   |
| timestamp  | LONG   | YES       | Current unix timestamp(ms)                   |
| recvWindow | LONG   | NO        | RecvWindow for this request.                 |

**Response:**

| Name              | type    | example         | description             |
|-------------------|---------|-----------------|-------------------------|
| `symbolId`        | string  | `BTC-PERP-USDT` | name of the contract    |
| `riskLimitId`     | string  | `1`             | risk limit id           |
| `riskLimitAmount` | string  | `1000000`       | risk limit amount       |
| `maintainMargin`  | string  | `0.005`         | maintenance margin rate |
| `initialMargin`   | string  | `0.01`          | initial margin rate     |
| `isLong`          | boolean | `true`          | position side           |

**Example:**
```json
[
    {
        "symbolId": "BTC-PERP-USDT",
        "riskLimits": [
            {
                "riskLimitId": "1",
                "riskLimitAmount": "1000000.0",
                "maintainMargin": "0.005",
                "initialMargin": "0.01",
                "isLong": false
            },
            {
                "riskLimitId": "1",
                "riskLimitAmount": "1000000.0",
                "maintainMargin": "0.005",
                "initialMargin": "0.01",
                "isLong": true
            }
        ]
    }
]
```

**Request Weight:**
1

#### Set Risk Limit

```shell
POST /openapi/contract/v1/setRiskLimit
```

Set risk limit on the symbol

**Headers:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
X-ACE-KEY | STRING | YES | Your API key

**Query Parameters:**

name|type| required |description
------------ | ------------ |----------| ------------
`symbol`|string|`YES`|Name of the contract
`riskLimitId`|integer|`YES`|risk limit id E.g. `1`
`isLong`|boolean|`YES`|position side

**Body Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
signature | STRING | YES | Authentication is needed for this endpoint
timestamp | LONG | YES | Current unix timestamp(ms)
recvWindow | LONG | NO | RecvWindow for this request.

**Example:**
```js
{
    "success": "true"
}
```

**Request Weight:**
1

#### Modify margin

```shell
POST /openapi/contract/v1/modifyMargin
```

Modify margin for a specific symbol

**Headers:**

| Name      | Type   | Mandatory | Description  |
|-----------|--------|-----------|--------------|
| X-ACE-KEY | STRING | YES       | Your API key |

**Query Parameters:**

| name     | type   | required | description                                                                                                                                                |
|----------|--------|----------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `symbol` | string | `NO`     | The symbol's margin to be modified. E.g. `BTC-PERP-USDT`                                                                                                   |
| `side`   | string | `YES`    | `LONG` or `SHORT`. Direction of the position                                                                                                               |
| `amount` | float  | `YES`    | Amount of margin to be added (Positive Value) or removed (Negative Value). Please note that this amount refers to the underlying quote asset of the asset. |

**Body Parameters:**

| Name       | Type   | Mandatory | Description                                |
|------------|--------|-----------|--------------------------------------------|
| signature  | STRING | YES       | Authentication is needed for this endpoint |
| timestamp  | LONG   | YES       | Current unix timestamp(ms)                 |
| recvWindow | LONG   | NO        | RecvWindow for this request.               |

**Response:**

| Name        | type   | example           | description                    |
|-------------|--------|-------------------|--------------------------------|
| `symbol`    | string | `BTC-PERP-USDT`   | The name of the contract.      |
| `margin`    | float  | `12.3`            | Updated margin for the symbol. |
| `timestamp` | long   | `1541161088303`   | Updated timestamp              |

**Example:**
```json
{
  "symbol": "BTC-PERP-USDT",
  "margin": 15,
  "timestamp": 1541161088303
}
```

**Request Weight:**
1

---

### Account

Security Type: **[USER_DATA/TRADE](02-general-information.md#endpoint-security-type)**

Endpoints under **Account** require an **[API-key and a signature](02-general-information.md#signed-trade-and-user_data-endpoint-security)**.

#### Account information

```shell
GET  /openapi/contract/v1/account
```

Get current contract account information (balances)

**Headers:**

| Name      | Type   | Mandatory | Description  |
|-----------|--------|-----------|--------------|
| X-ACE-KEY | STRING | YES       | Your API key |

**Query Parameters:**
None

**Body Parameters:**

| Name       | Type   | Mandatory | Description                                |
|------------|--------|-----------|--------------------------------------------|
| signature  | STRING | YES       | Authentication is needed for this endpoint |
| timestamp  | LONG   | YES       | Current unix timestamp(ms)                 |
| recvWindow | LONG   | NO        | RecvWindow for this request.               |

**Response:**

| Name              | type  | example        | description                    |
|-------------------|-------|----------------|--------------------------------|
| `total`           | float | `131.06671401` | Total balance.                 |
| `availableMargin` | float | `131.0545541`  | Available margin for use.      |
| `positionMargin`  | float | `0.01215991`   | Margin for positions.          |
| `orderMargin`     | float | `0`            | Margin locked for open orders. |

**Example:**

```json
{
  "BTC": {
    "total":"131.06671401",
    "availableMargin":"131.0545541",
    "positionMargin":"0.01215991",
    "orderMargin":"0"
  },...
}
```

**Request Weight:**
5

```json
{
  "message": "success",
  "timestamp": 1541161088303
}
```

