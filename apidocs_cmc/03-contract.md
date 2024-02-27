# CMC | API Endpoint summary

## **[SECTION B] Derivative Exchanges**

ENDPOINT OVERVIEW

| Name        | Category  | Status    | Description                                                                                                                                                                 |
|-------------|-----------|-----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Endpoint B1 | contracts | Mandatory | Summary of contracts traded on the exchange, helps to differentiate between different products available. Ideally, all information should be returned in a single endpoint. |
| Endpoint B2 | orderbook | Mandatory | Order book depth of any given trading pair, split into two different arrays for bid and ask orders.                                                                         |

**Rate limiters (rateLimitType)**

- REQUESTS_WEIGHT

<br>

**Rate limit intervals**

- MINUTE

For example:

```json
{
  "rateLimitType": "REQUEST_WEIGHT",
  "interval": "MINUTE",
  "limit": 1500
}

```

<br>

### **Public**

Security Type: **None**

Endpoints under **Public** section can be accessed freely without requiring any API-key or signatures.

---

**Contract Endpoint**

```shell
GET /openapi/cmc/v1/contracts
```

Get summary of every single contract traded on the exchange.

**Weight:** 1

**Parameters**: None

**Response**:

| Name                        | Type                          | Status                                          | Description                                                                                             |
|-----------------------------|-------------------------------|-------------------------------------------------|---------------------------------------------------------------------------------------------------------|
| ticker_id                   | string                        | Mandatory                                       | Identifier of a ticker with delimiter to separate base/quote, eg. BTC-PERPUSD, BTC-PERPETH, BTC-PERPEUR |
| base_currency               | string                        | Mandatory                                       | Symbol/currency code of base pair, eg. BTC                                                              |
| quote_currency              | string                        | Mandatory                                       | Symbol/currency code of quote pair, eg. ETH                                                             |
| last_price                  | decimal                       | Mandatory                                       | Last transacted price of base currency based on given quote currency                                    |
| base_volume                 | decimal                       | Mandatory                                       | 24 hour trading volume in BASE currency                                                                 |
| quote_volume                | decimal                       | Mandatory                                       | 24 hour trading volume in QUOTE currency                                                                |
| bid                         | decimal                       | Mandatory                                       | Current highest bid price                                                                               |
| ask                         | decimal                       | Mandatory                                       | Current lowest ask price                                                                                |
| high                        | decimal                       | Mandatory                                       | Rolling 24-hour highest transaction price                                                               |
| low                         | decimal                       | Mandatory                                       | Rolling 24-hour lowest transaction price                                                                |
| product_type                | string                        | Mandatory                                       | Futures, Perpetual, Options                                                                             |
| open_interest               | decimal                       | Mandatory                                       | The number of outstanding derivatives  contracts that have not been settled                             |
| open_interest_usd           | decimal                       | Mandatory                                       | The sum of the Open Positions (long or short) in USD Value of the contract                              |
| index_price                 | decimal                       | Mandatory                                       | Last calculated index price for underlying of contract                                                  |
| funding_rate                | decimal                       | Mandatory                                       | Current funding rate                                                                                    |
| next_funding_rate           | decimal                       | Recommended                                     | Upcoming predicted funding rate                                                                         |
| next_funding_rate_timestamp | Integer (UTC timestamp in ms) | Mandatory                                       | Timestamp of the next funding rate change                                                               |
| creation_timestamp          | Integer (UTC timestamp in ms) | Mandatory (only for expirable futures/options)  | Start date of derivative (not needed for perpetual swaps)                                               |
| expiry_timestamp            | Integer (UTC timestamp in ms) | Mandatory (only for expirable futures/options)  | End date of derivative (not needed for perpetual swaps)                                                 |
| contract_type               | string                        | Mandatory                                       | Describes the type of contract - Vanilla, Inverse or Quanto?                                            |
| contract_price              | decimal                       | Mandatory                                       | Describes the price per contract.                                                                       |
| contract_price_currency     | string                        | Mandatory                                       | Describes the currency which the contract is priced in (e.g. USD, EUR, BTC, USDT)                       |

**ENDPOINT SAMPLE (CONTRACTS)**

```json
[
    {
        "ticker_id": "BTC-PERPUSDT",
        "base_currency": "BTC",
        "quote_currency": "USDT",
        "last_price": 56439,
        "base_volume": 85733016,
        "quote_volume": 465025037.0056,
        "bid": 56435.5,
        "ask": 56437,
        "high": 57512,
        "low": 50936,
        "product_type": "Perpetual",
        "open_interest": 21947,
        "open_interest_usd": 123747.830295,
        "index_price": 56384.85,
        "funding_rate": -0.00129,
        "next_funding_rate": -0.00129,
        "next_funding_rate_timestamp": 1709020800,
        "creation_timestamp": 1708929596,
        "expiry_timestamp": 1709015996,
        "contract_type": "Vanilla",
        "contract_price": 56439,
        "contract_price_currency": "USDT"
    },
    {
        "ticker_id": "ETH-PERPUSDT",
        "base_currency": "ETH",
        "quote_currency": "USDT",
        "last_price": 3237.05,
        "base_volume": 3302452,
        "quote_volume": 104739184.66325,
        "bid": 3236.45,
        "ask": 3236.9,
        "high": 3290.55,
        "low": 3039.2,
        "product_type": "Perpetual",
        "open_interest": 0,
        "open_interest_usd": 0,
        "index_price": 3234.695,
        "funding_rate": 0.00028572,
        "next_funding_rate": 0.00089113,
        "next_funding_rate_timestamp": 1709020800,
        "creation_timestamp": 1708929595,
        "expiry_timestamp": 1709015995,
        "contract_type": "Vanilla",
        "contract_price": 3237.05,
        "contract_price_currency": "USDT"
    }
]
```

---

**Orderbook Endpoint**

```shell
GET /openapi/cmc/v1/order_book
```

Get order book information with maximum dept = 100 returned for a given market pair/ticker

**Weight:**:1

**Parameters:**

| Name       | Type     | Status       | Description                                                                                                                        |
|------------|----------|--------------|------------------------------------------------------------------------------------------------------------------------------------|
| ticker_id  | string   | Recommended  | Identifier of a ticker with delimiter to separate base/quote, eg. BTC-PERPUSD, BTC-PERPETH, BTC-PERPEUR. Not defined = all ticker. |
| depth      | integer  | Recommended  | Depth = 10 means 10 for each bid/ask side. Not defined = full orderbook.                                                           |

**Response:**

| Name       | Type                           | Status    | Description                                                                      |
|------------|--------------------------------|-----------|----------------------------------------------------------------------------------|
| ticker_id  | string                         | Mandatory | A pair such as "BTC-PERPUSD", with delimiter between different cryptoassets      |
| timestamp  | integer (UTC timestamp in ms)  | Mandatory | Unix timestamp in milliseconds for when the last updated time occurred.          |
| bids       | decimal                        | Mandatory | An array containing 2 elements. The offer price and quantity fyor each bid order |
| asks       | decimal                        | Mandatory | An array containing 2 elements. The ask price and quantity for each ask order    |

**ENDPOINT SAMPLE (ORDERBOOK)**

```json
[
    {
        "ticker_id": "BTC-PERPUSDT",
        "timestamp": 1709018961540,
        "bids": [
            [
                56036,
                2678
            ],
            [
                56033.5,
                5596
            ],
            [
                56033,
                6.18E+3
            ],
            [
                56032.5,
                6764
            ],
            [
                56032,
                5983
            ],
            [
                56031.5,
                5.07E+3
            ],
            [
                56031,
                7167
            ],
            [
                56030.5,
                5216
            ],
            [
                5.603E+4,
                986
            ],
            [
                56029.5,
                6338
            ]
        ],
        "asks": [
            [
                56037.5,
                2.61E+3
            ],
            [
                5.604E+4,
                3183
            ],
            [
                56040.5,
                3047
            ],
            [
                56041,
                1859
            ],
            [
                56041.5,
                2067
            ],
            [
                56042,
                2903
            ],
            [
                56042.5,
                2589
            ],
            [
                56043,
                3335
            ],
            [
                56043.5,
                2.57E+3
            ],
            [
                56044,
                2313
            ]
        ]
    }
]
```