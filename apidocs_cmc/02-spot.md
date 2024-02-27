# CMC | API Endpoint summary

## [SECTION A] Spot Exchanges
ENDPOINT OVERVIEW

Name             | Category   | Status      | Description
-----------------|------------|-------------|-----------------------------------------------------------------------------------------------------------------------------
Summary Endpoint | Summary    | Mandatory   | Overview of market data for all tickers and all markets.
Endpoint A1      | asset      | Mandatory   | In depth details on cryptocurrencies available on the exchange.
Endpoint A2      | ticker     | Mandatory   | 24-hour rolling window price change statistics for all markets.
Endpoint A3      | orderbook  | Mandatory   | Market depth of a trading pair. One array containing a list of ask prices and another array containing bid prices. Query for level 2 order book with full depth available as minimum requirement.
Endpoint A4      | trades     | Mandatory   | Recently completed trades for a given market. 24 hour historical full trades available as minimum requirement.


**Rate limiters (rateLimitType)**
* REQUESTS_WEIGHT

<br>

**Rate limit intervals**

* MINUTE

For example:
```json
{
  "rateLimitType": "REQUEST_WEIGHT",
  "interval": "MINUTE",
  "limit": 1500
}
```
<br>

### Public

Security Type: **None**

Endpoints under **Public** section can be accessed freely without requiring any API-key or signatures.

---
#### Summary Endpoint

```shell
GET /openapi/quote/cmc/v1/summary
```
Overview of market data for all tickers and all markets.

**Weight:**
1

**Parameters:**
None

**Response:**

Name | Type   | Status | Description                                                                                                                                                     
------|--------|------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------
trading_pairs | string | Mandatory     | Identifier of a ticker with delimiter to separate base/quote, eg. BTC-USD (Price of BTC is quoted in USD).
base_currency | string | Recommended     | Symbol/currency code of base currency, eg. BTC
quote_currency | string | Recommended     | Symbol/currency code of quote currency, eg. USD
last_price | decimal | Mandatory     | Last transacted price of base currency based on given quote currency
lowest_ask | decimal | Mandatory     | Lowest Ask price of base currency based on given quote currency
highest_bid | decimal | Mandatory     | Highest bid price of base currency based on given quote currency
base_volume | decimal | Mandatory     | 24-hr volume of market pair denoted in BASE currency
quote_volume | decimal | Mandatory     | 24-hr volume of market pair denoted in QUOTE currency
price_change_percent_24h | decimal | Mandatory     | 24-hr % price change of market pair
highest_price_24h | decimal | Mandatory     | Highest price of base currency based on given quote currency in the last 24-hrs
lowest_price_24h | decimal | Mandatory     | Lowest price of base currency based on given quote currency in the last 24-hrs

**ENDPOINT SAMPLE (SUMMARY)**

```json
[
  {
    "trading_pairs": "BTC_USDT",
    "base_currency": "BTC",
    "quote_currency": "USDT",
    "last_price": "52101.5",
    "lowest_ask": "52101.5",
    "highest_bid": "52101",
    "base_volume": "1696.2508275",
    "quote_volume": "88001450.1765415",
    "price_change_percent_24h": "0.0085",
    "highest_price_24h": "52978",
    "lowest_price_24h": "50761.5"
  },
  {
    "trading_pairs": "ADA_USDT",
    "base_currency": "ADA",
    "quote_currency": "USDT",
    "last_price": "0.6083",
    "lowest_ask": "0.6089",
    "highest_bid": "0.6085",
    "base_volume": "1264136.255",
    "quote_volume": "785234.685759",
    "price_change_percent_24h": "-0.0405",
    "highest_price_24h": "0.6415",
    "lowest_price_24h": "0.5962"
  }
]
```
---


**Assets Endpoint**

```shell
GET /openapi/quote/cmc/v1/assets
```

In depth details on cryptocurrencies available on the exchange.

**Weight:** 1

**Parameters:** None

**Response:**

| Name                   | Type    | Status      | Description                                                          |
|------------------------|---------|-------------|----------------------------------------------------------------------|
| name                   | string  | Recommended | Full name of cryptocurrency.                                         |
| unified_cryptoasset_id | integer | Recommended | Unique ID of cryptocurrency assigned by Unified Cryptoasset ID.      |
| can_withdraw           | boolean | Recommended | Identifies whether withdrawals are enabled or disabled.              |
| can_deposit            | boolean | Recommended | Identifies whether deposits are enabled or disabled.                 |
| min_withdraw           | decimal | Recommended | Identifies the single minimum withdrawal amount of a cryptocurrency. |
| maker_fee              | decimal | Recommended | Fees applied when liquidity is added to the order book.              |
| taker_fee              | decimal | Recommended | Fees applied when liquidity is removed from the order book.          |

**ENDPOINT SAMPLE (ASSETS)**

```json
{
    "SHIB": {
        "name": "Shiba Inu",
        "unified_cryptoasset_id": 5994,
        "can_withdraw": false,
        "can_deposit": true,
        "min_withdraw": "1200000",
        "taker_fee": "0.001",
        "maker_fee": "0.001"
    },
    "DOGE": {
        "name": "Dogecoin",
        "unified_cryptoasset_id": 74,
        "can_withdraw": false,
        "can_deposit": true,
        "min_withdraw": "300",
        "taker_fee": "0.001",
        "maker_fee": "0.001"
    },
    "USDT": {
        "name": "Tether",
        "unified_cryptoasset_id": 825,
        "can_withdraw": true,
        "can_deposit": true,
        "min_withdraw": "20",
        "taker_fee": "0.001",
        "maker_fee": "0.001"
    },
    "BTC": {
        "name": "Bitcoin",
        "unified_cryptoasset_id": 1,
        "can_withdraw": false,
        "can_deposit": true,
        "min_withdraw": "0.0003",
        "taker_fee": "0.001",
        "maker_fee": "0.001"
    }
}
```

---

**Ticker Endpoint**

```shell
GET /openapi/quote/cmc/v1/ticker
```

24-hour rolling window price change statistics for all markets.

**Weight:** 1

**Parameters:** None

**Response:**

| Name         | Type     | Status      | Description                                                          |
|--------------|----------|-------------|----------------------------------------------------------------------|
| last_price   | decimal  | Mandatory   | Last transacted price of base currency based on given quote currency |
| base_volume  | decimal  | Mandatory   | 24-hour trading volume denoted in BASE currency                      |
| quote_volume | decimal  | Mandatory   | 24 hour trading volume denoted in QUOTE currency                     |
| base_id      | integer  | Recommended | The quote pair Unified Cryptoasset ID.                               |
| quote_id     | integer  | Recommended | The base pair Unified Cryptoasset ID.                                |

**ENDPOINT SAMPLE (TICKER)**

```json
{
    "ETH_USDT": {
        "trading_pairs": "ETH_USDT",
        "last_price": "3220.25",
        "base_volume": "35709.35109",
        "quote_volume": "113042132.4151945",
        "base_id": 1027,
        "quote_id": 825
    },
    "TRX_USDT": {
        "trading_pairs": "TRX_USDT",
        "last_price": "0.14065",
        "base_volume": "17361710",
        "quote_volume": "2420095.031769",
        "base_id": 1958,
        "quote_id": 825
    },
    "DOGE_USDT": {
        "trading_pairs": "DOGE_USDT",
        "last_price": "0.0914",
        "base_volume": "797994816.8",
        "quote_volume": "67504061.28776",
        "base_id": 74,
        "quote_id": 825
    },
    "AVAX_USDT": {
        "trading_pairs": "AVAX_USDT",
        "last_price": "39.16",
        "base_volume": "95291.72015",
        "quote_volume": "3662693.145203",
        "base_id": 5805,
        "quote_id": 825
    },
    "BTC_USDT": {
        "trading_pairs": "BTC_USDT",
        "last_price": "56083.5",
        "base_volume": "3636.413952",
        "quote_volume": "198606120.144424",
        "base_id": 1,
        "quote_id": 825
    },
    "SHIB_USDT": {
        "trading_pairs": "SHIB_USDT",
        "last_price": "0.00001029",
        "base_volume": "59148804499.6",
        "quote_volume": "585275.955907204",
        "base_id": 5994,
        "quote_id": 825
    },
    "XRP_USDT": {
        "trading_pairs": "XRP_USDT",
        "last_price": "0.551",
        "base_volume": "7684097.491",
        "quote_volume": "4195441.8499595",
        "base_id": 52,
        "quote_id": 825
    },
    "SOL_USDT": {
        "trading_pairs": "SOL_USDT",
        "last_price": "110.83",
        "base_volume": "142970.996",
        "quote_volume": "15325578.3764",
        "base_id": 5426,
        "quote_id": 825
    }
}
```

---

#### Orderbook Endpoint

```shell
GET /openapi/quote/cmc/v1/orderbook
```
Market depth of a trading pair. One array containing a list of ask prices and another array containing bid prices. Query for level 2 order book with full depth available as minimum requirement.

**Weight:**
1

**Parameters:**

Name | Type   | Status    | Description                                                                                                                                                     
------|--------|-----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------
market_pair | string | Mandatory | A pair such as “BTC_USDT”
depth | int | Recommended (used to calculate liquidity score for rankings) | Orders depth quantity: [0,10,20,50,100,500] Not defined or 0 = full order book Depth = 10 means 5 for each bid/ask side.




**Response:**

Name | Type                          | Status    | Description                                                                                                                                                     
------|-------------------------------|-----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------
timestamp | Integer (UTC timestamp in ms) | Mandatory | Unix timestamp in milliseconds for when the last updated time occurred.
bids | decimal                           | Mandatory | An array containing 2 elements. The offer price and quantity for each bid order.
asks | decimal                           | Mandatory | An array containing 2 elements. The ask price and quantity for each ask order.

**ENDPOINT SAMPLE (ORDERBOOK)**
```json
{
  "timestamp": 1708487600749,
  "bids": [
    [
      "52024",
      "0.021317"
    ],
    [
      "52021.5",
      "0.383861"
    ],
    [
      "52021",
      "0.206568"
    ],
    [
      "52020.5",
      "0.340698"
    ],
    [
      "52020",
      "0.384111"
    ]
  ],
  "asks": [
    [
      "52024.5",
      "0.1114365"
    ],
    [
      "52027",
      "0.4038945"
    ],
    [
      "52027.5",
      "0.4667575"
    ],
    [
      "52028",
      "0.496174"
    ],
    [
      "52028.5",
      "0.5567425"
    ]
  ]
}

```

---

#### Trades Endpoint

```shell
GET /openapi/quote/cmc/v1/trades
```
Recently completed trades for a given market. 24 hour historical full trades available as minimum requirement.

**Weight:**
1

**Parameters:**

Name | Type   | Status    | Description                                                                                                                                                     
------|--------|-----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------
market_pair | string | Mandatory | A pair such as BTC_USDT.




**Response:**

Name | Type     | Status    | Description                                                                                                                                                     
------|----------|-----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------
price | decimal  | Mandatory | Last transacted price of base currency based on given quote currency.
trade_id | integer  | Mandatory | A unique ID associated with the trade for the currency pair transaction Note: Unix timestamp does not qualify as trade_id.
base_volume | decimal  | Mandatory | Transaction amount in BASE currency.
quote_volume | decimal  | Mandatory | Transaction amount in QUOTE currency.
timestamp | decimal  | Mandatory | Unix timestamp in milliseconds for when the transaction occurred.
type | string | Mandatory | Used to determine whether or not the transaction originated as a buy or sell.

**ENDPOINT SAMPLE (TRADES)**
```json
[
  {
    "price": "52009.5",
    "trade_id": 1625312232823079000,
    "base_volume": "0.0001085",
    "quote_volume": "5.64303075",
    "timestamp": 1708488316573,
    "type": "Sell"
  },
  {
    "price": "52010",
    "trade_id": 1625312243476611000,
    "base_volume": "0.0002735",
    "quote_volume": "14.224735",
    "timestamp": 1708488317843,
    "type": "Buy"
  },
  {
    "price": "52010",
    "trade_id": 1625312259515629600,
    "base_volume": "0.000257",
    "quote_volume": "13.36657",
    "timestamp": 1708488319755,
    "type": "Buy"
  },
  {
    "price": "52005.5",
    "trade_id": 1625312276972322800,
    "base_volume": "0.00005",
    "quote_volume": "2.600275",
    "timestamp": 1708488321836,
    "type": "Sell"
  },
  {
    "price": "52006",
    "trade_id": 1625312280151605200,
    "base_volume": "0.0002855",
    "quote_volume": "14.847713",
    "timestamp": 1708488322215,
    "type": "Buy"
  },
  {
    "price": "52004.5",
    "trade_id": 1625312295402094600,
    "base_volume": "0.000243",
    "quote_volume": "12.6370935",
    "timestamp": 1708488324033,
    "type": "Sell"
  }
]


```

---