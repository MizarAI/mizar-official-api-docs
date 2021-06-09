<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Public Rest API for mizar.ai](#public-rest-api-for-mizarai)
- [General API for Mizar (2021-06-07)](#general-api-for-mizar-2021-06-07)
- [HTTP Return Codes](#http-return-codes)
- [Data Sources](#data-sources)
- [Endpoint security type](#endpoint-security-type)
- [Public API Endpoints](#public-api-endpoints)
  - [General endpoints](#general-endpoints)
    - [Test connectivity](#test-connectivity)
    - [Check server time](#check-server-time)
  - [Exchange data endpoints](#exchange-data-endpoints)
    - [Get exchange information and available markets (EXCHANGES_DATA)](#get-exchange-information-and-available-markets-exchanges_data)
    - [Available markets per exchange (EXCHANGES_DATA)](#available-markets-per-exchange-exchanges_data)
    - [Available bar types (EXCHANGES_DATA)](#available-bar-types-exchanges_data)
    - [Bar data (EXCHANGES_DATA)](#bar-data-exchanges_data)
  - [Hosted strategies endpoints](#hosted-strategies-endpoints)
    - [Publish Strategy](#publish-strategy)
    - [Hosted strategy info](#hosted-strategy-info)
    - [Hosted strategy positions](#hosted-strategy-positions)
    - [Hosted strategy live trading stats](#hosted-strategy-live-trading-stats)
    - [Hosted strategy paper trading stats](#hosted-strategy-paper-trading-stats)
  - [Self-hosted fund endpoints](#self-hosted-fund-endpoints)
    - [Publish self-hosted fund](#publish-self-hosted-fund)
    - [Open position](#open-position)
    - [close position](#close-position)
    - [close all positions](#close-all-positions)
    - [Fund info](#fund-info)
    - [Fund positions](#fund-positions)
    - [Fund live trading stats](#fund-live-trading-stats)
    - [Fund paper trading stats](#fund-paper-trading-stats)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Public Rest API for [mizar.ai](https://mizar.ai/) 
# General API for Mizar (2021-06-09)

* The base endpoint is: **https://api.mizar.ai/**
* All endpoints return either a JSON object or array.
* Data is returned in **ascending** order. Oldest first, newest last.
* All time and timestamp related fields are in **milliseconds**.

# HTTP Return Codes

* HTTP `4XX` return codes are used for malformed requests; the issue is on the sender's side.
* HTTP `429` return code is used when breaking a request rate limit.
* HTTP `5XX` return codes are used for internal errors; the issue is on Mizar's side.

# Data Sources

* Each endpoint has a data source indicating where the data is being retrieved.
These are the three sources:
    * **Memory** - the data is from a server's local or external memory
    * **Database** - the data is taken directly from a database
    
Some endpoints retrieve data from Memory and/or Database.

# Endpoint security type
* Each endpoint has a security type that determines how you will interact with it. This is stated next to the NAME of the endpoint.
    * If no security type is stated, assume the security type is NONE.
* API-key are passed into the Rest API via the `mizar-api-key` header.
* The API-key **is case-sensitive**.

Security Type | Description
------------ | ------------
NONE | Endpoint can be accessed freely.
QUANT | Endpoint requires sending a valid API-Key.
FUND | Endpoint requires sending a valid API-Key.

EXCHANGES_DATA | Endpoint requires sending a valid API-Key.

# Public API Endpoints

## General endpoints
### Test connectivity

```
GET /api/v1/ping
```
Test connectivity to the Rest API

**Weight:**
1

**Parameters:**
NONE

**Data Source:**
Memory

**Response:**
```javascript
{}
```

### Check server time

```
GET /api/v1/server-time
```

Test connectivity to the Rest API and get the current server time.

**Weight:**
1

**Parameters:**
NONE

**Data Source:**
Memory

**Response:**
```javascript
{"server_time": 1623081868169}
```

## Exchange data endpoints
### Get exchange information and available markets (EXCHANGES_DATA)

```
GET /api/v1/exchanges
```

**Weight:**
1

**Parameters:**
NONE

**Data Source:**
Database

**Response:**
```javascript
{
    "exchanges":
    [
        {
            "markets": [
                "spot"
            ],
            "name": "binance"
        }
    ]
}
```

### Available markets per exchange (EXCHANGES_DATA)

```
GET /api/v1/markets
```

**Weight:**
1

**Parameters:**

Name | Type | Mandatory | Values(default) | Description
------------ | ------------ | ------------ | ------------ | ------------
exchange | string | YES |   | exchange from GET /v1/exchanges

**Data Source:**
Database

**Response:**
```javascript
{
  "markets":
  [
    {
      "quote_asset": "USDT",
      "base_asset": "BTC",
      "markets": ["spot"]
    },
  ]
}
```


### Available bar types (EXCHANGES_DATA)

```
GET /api/v1/bar-types
```

**Weight:**
1

**Parameters:**

Name | Type | Mandatory | Values(default) | Description
------------ | ------------ | ------------ | ------------ | ------------
base_asset | str | NO |    | base_asset from GET /v1/base-assets
quote_asset | str | NO |    | quote_asset from GET /v1/quote-assets
bar_type | str | NO |    | bar_type from GET /v1/bar-types
bar_subclass | str | NO |    | bar_subclass from GET /v1/bar-types
exchange | string | YES |   | exchange from GET /v1/exchanges

**Data Source:**
Database

**Response:**
```javascript
{
  "bar_types":
  [
    {
      "bar_subclass": "5min",
      "bar_type": "time",
      "base_asset": "ADA",
      "exchange": "binance",
      "quote_asset": "USDT"
    },
    {
      "bar_subclass": "12H",
      "bar_type": "time",
      "base_asset": "ADA",
      "exchange": "binance",
      "quote_asset": "USDT"
    },
    {
      "bar_subclass": "1H",
      "bar_type": "time",
      "base_asset": "ADA",
      "exchange": "binance",
      "quote_asset": "USDT"
    }
  ]
}
```

### Bar data (EXCHANGES_DATA)

```
GET /api/v1/bars
```

**Weight:**
1

**Parameters:**

Name | Type | Mandatory | Values(default) | Description
------------ | ------------ | ------------ | ------------ | ------------
base_asset | str | YES |    | base_asset from GET /v1/base-assets
quote_asset | str | YES |    | quote_asset from GET /v1/quote-assets
start_timestamp | int | YES |    | timestamp in seconds
limit | int | YES |    | Default 500; max 500
bar_type | str | YES |    | bar_type from GET /v1/bar-types
bar_subclass | str | YES |    | bar_subclass from GET /v1/bar-types
exchange | string | YES |   | exchange from GET /v1/exchanges

**Data Source:**
Database

**Response:**
```javascript
{
  "bars":
  [
    {
      "base_asset_buy_volume": 616.248541000001,
      "base_asset_sell_volume": 178.901836,
      "base_asset_volume": 795.150376999997,
      "close": 4285.08,
      "first_timestamp": 1502942428322,
      "first_trade_id": 0,
      "high": 4485.39,
      "last_timestamp": 1503014320719,
      "last_trade_id": 3426,
      "low": 4200.74,
      "num_buy_ticks": 2372,
      "num_sell_ticks": 1055,
      "num_ticks": 3427,
      "open": 4261.48,
      "quote_asset_buy_volume": 2678216.400604011,
      "quote_asset_sell_volume": 776553.6501280505,
      "quote_asset_volume": 3454770.050732071,
      "time": 1502928000000
    },
    {
      "base_asset_buy_volume": 972.868709999997,
      "base_asset_sell_volume": 227.019554000001,
      "base_asset_volume": 1199.888264000002,
      "close": 4108.37,
      "first_timestamp": 1503014448072,
      "first_trade_id": 3427,
      "high": 4371.52,
      "last_timestamp": 1503100748883,
      "last_trade_id": 8659,
      "low": 3938.77,
      "num_buy_ticks": 4067,
      "num_sell_ticks": 1166,
      "num_ticks": 5233,
      "open": 4285.08,
      "quote_asset_buy_volume": 4129123.316518072,
      "quote_asset_sell_volume": 957834.9896534317,
      "quote_asset_volume": 5086958.30617149,
      "time": 1503014400000
    }
}
```

## Hosted strategies endpoints

### Publish Strategy (QUANT)

```
POST /api/v1/publish-hosted-strategy
```

**Weight:**
1

**Data:**

Name | Type | Mandatory | Values(default) | Description
------------ | ------------ | ------------ | ------------ | ------------
strategy_info | json | YES |   |
strategy | str | YES |    |
strategy_file | str | YES |    |

**strategy_info:**

Name | Type | Mandatory | Values(default) | Description
------------ | ------------ | ------------ | ------------ | ------------
name | str | YES |   | strategy name
target_data_source | str | YES |    | The name of the target data source
num_expiration_bars | int | YES |    | number of bars after which the positions created by the strategy are expired
data_sources | json | YES |    | data sources needed for the strategy
description | str | YES |    | strategy description
trailing_take_profit_deviation | float | NO | the trailing take profit of the strategy
trailing_stop_loss_deviation | float | NO | the trailing stop loss of the strategy
take_profit_tick_level | bool | YES | whether the take profit is triggered by ticks (if false the take profit is triggered by the close value)
stop_loss_tick_level | bool | YES | whether the stop loss is triggered byt ticks (if false the stop loss is triggered by the close value)

**Response:**
```javascript
{
    "strategy_signal_name": "My strategy",
    "target_data_source": "btc_usd_1d",
    "num_expiration_bars": 20,
    "data_sources": {
        "btc_usd_1d": {
              "data_type": "bar", 
              "quote_asset": "USDT",
              "base_asset": "BTC",
              "exchange": "binance",
              "bar_transformer_class": "time",
              "bar_transformer_subclass" :"D"
        }
    },
    "description": "My profitable strategy works this way",
    "trailing_take_profit_deviation": 0.1,
    "trailing_stop_loss_deviation": 0.1,
    "stop_loss_tick_level": false,
    "take_profit_tick_level": false,
}
```

**Data Source:**
None

```javascript
{
  "message": "The strategy <Strategy name> has been published in Mizar"
}
```

### Hosted strategy info (QUANT)

```
GET /api/v1/hosted-strategies-info
```

**Weight:**
1

**Parameters:**

Name | Type | Mandatory | Values(default) | Description
------------ | ------------ | ------------ | ------------ | ------------
base_asset | str | NO |    | base_asset from GET /v1/base-assets
quote_asset | str | NO |    | quote_asset from GET /v1/quote-assets
bar_type | str | NO |    | bar_type from GET /v1/bar-types
bar_subclass | str | NO |    | bar_subclass from GET /v1/bar-types
exchange | string | YES |   | exchange from GET /v1/exchanges

**Data Source:**
Database

**Response:**
```javascript
{
  "hosted_strategies":
  [
    {
      "name": "My strategy",
      "version": 0,
      "is_valid": true,
      "is_public": true,
      "is_backtested": true,
      "active_subscribers": 1000,
      "profit_fee": 0.1,
      "published_timestamp": 11111111111,
    },
  ]
}
```

### Hosted strategy positions (QUANT)

```
GET /api/v1/hosted-strategy-positions
```

**Weight:**
1

**Parameters:**

Name | Type | Mandatory | Values(default) | Description
------------ | ------------ | ------------ | ------------ | ------------
name | str | YES |    | name from GET api/v1/hosted-strategies-info
version | int | YES |    | version from GET api/v1/hosted-strategies-info

**Data Source:**
Memory, Database

**Response:**
```javascript
{
  "positions": [
    {   
        "is_closed": true,
        "open_timestamp": 1623081868169,
        "closed_timestamp": 1623081968169,
        "open_price": 30000,
        "close_price": 100000,
        "size": 1.0,
        "is_long": true,
        "expiration_bar": false,
        "profit_taking_bar": true,
        "stop_loss_bar": false,
        "rule": false,
        "profit_taking_value": 100000,
        "stop_loss_value": 77771
    },
  ]
}
```

### Hosted strategy live trading stats (QUANT)
```
GET /api/v1/hosted-strategy-live-trading-stats
```

**Weight:**
1

**Parameters:**

Name | Type | Mandatory | Values(default) | Description
------------ | ------------ | ------------ | ------------ | ------------
name | str | YES |    | name from GET api/v1/hosted-strategies-info
version | int | YES |    | version from GET api/v1/hosted-strategies-info

**Data Source:**
Memory, Database

**Response:**
```javascript
{
  "strategy-live-stats": {
      "active_subscribers": 1000,
      "aum_quote": 100,
      "aum_base": 100,
      "avg_returns_per_trade": 0.05, 
      "best_won_quote": 0.10,
      "worst_loss_quote": -0.10,
      "best_won_base": 0.10,
      "worst_loss_base": 0.10, 
      "total_traded_volume_quote": 100,
      "total_traded_volume_base": 100,
      "current_month_traded_volume_quote": 50,
      "current_month_traded_volume_base": 50,
      "current_month_pnl_quote": 2,
      "current_month_pnl_base": 1.5,
      "total_shared_profits": 100,
      "current_month_estiamted_shared_profits": 1000 
    }
}
```

### Hosted strategy paper trading stats (QUANT)

```
GET /api/v1/hosted-strategy-paper-trading-stats
```

**Weight:**
1

**Parameters:**

Name | Type | Mandatory | Values(default) | Description
------------ | ------------ | ------------ | ------------ | ------------
name | str | YES |    | name from GET api/v1/hosted-strategies-info
version | int | YES |    | version from GET api/v1/hosted-strategies-info

**Data Source:**
Memory, Database

**Response:**
```javascript
{
  "strategy-paper-stats": {
      "total_n_trades": 100,
      "average_trade_per_week": 20,
      "avg_returns_per_trade": 0.05, 
      "best_won_quote": 0.10,
      "worst_loss_quote": -0.10,
      "best_won_base": 0.10
      "worst_loss_base": 0.10,
      "total_returns_quote": 0.2,
      "total_returns_base": 0.1,
      "current_month_returns_quote": 0.1,
      "current_month_returns_base": 0.05
    }
}
```

## Self-hosted fund endpoints

### Publish self-hosted fund (FUND)

```
POST /api/v1/publish-self-hosted-fund
```

**Weight:**
1

**Parameters:**

Name | Type | Mandatory | Values(default) | Description
------------ | ------------ | ------------ | ------------ | ------------
name | str | YES |   | strategy name
exchange | string | YES |   | exchange from GET api/v1/exchanges
markets | array | YES |   | example : ```javascript[{"quote_asset": "USDT", "base_asset": "BTC"}]```

**Data Source:**
None

**Response:**
```javascript
{
  "message": "The fund <Fund name> has been published in Mizar"
}
```

### Open position (FUND)

```
POST /api/v1/open-position
```

**Weight:**
1

**Parameters:**

Name | Type | Mandatory | Values(default) | Description
------------ | ------------ | ------------ | ------------ | ------------
base_asset | str | YES |   |
quote_asset | str | YES |   |
size | float | NO | 1.0 | the size of the position relative to the max size set by the user
is_long | bool | YES |    |  
stop_loss | float | NO |    | stop loss from opening price
take_profit | float | NO |    | take profit from opening price
trailing_stop_loss_deviation | float | NO |    | 
trailing_take_profit_deviation | float | NO |    |
expiration_time | int | NO |    | number of seconds before expiration

**Data Source:**
None

**Response:**

```javascript
{
  "message": "The position has been successfully open"
  "position_id" : 777
}
```

### Close position (FUND)

```
POST /api/v1/close-position
```

**Weight:**
1

**Parameters:**

Name | Type | Mandatory | Values(default) | Description
------------ | ------------ | ------------ | ------------ | ------------
position_id | int | YES |   |

**Data Source:**
None

**Response:**

```javascript
{
  "message": "The position <position_id> has been successfully closed"
}
```

### Close all positions (FUND)

```
POST /api/v1/close-all-positions
```

**Weight:**
1

**Parameters:**

Name | Type | Mandatory | Values(default) | Description
------------ | ------------ | ------------ | ------------ | ------------
fund_id | int | NO |   |   | fund_id from GET /api/v1/funds-info if not specified all the positions for all the fund will be closed

**Data Source:**
None

**Response:**
```javascript
{
  "message": "The position <position_id> has been successfully closed"
}
```

### Fund info (FUND)

```
GET /api/v1/funds-info
```

**Weight:**
1

**Parameters:**

NONE

**Data Source:**
Database

**Response:**
```javascript
{
  "self_hosted_funds":
  [
    {
      "id": 1,
      "name": "My fund",
      "active_subscribers": 1000,
      "profit_fee": 0.1,
      "published_timestamp": 1623081868169,
    },
  ]
}
```

### Fund positions (FUND)

```
GET /api/v1/hosted-fund-positions
```

**Weight:**
1

**Parameters:**

Name | Type | Mandatory | Values(default) | Description
------------ | ------------ | ------------ | ------------ | ------------
fund_id | int | YES |    | fund_id from GET /api/v1/funds-info

**Data Source:**
Memory, Database

**Response:**

```javascript
{
  "positions": [
    {   
        "base_asset": "BTC",
        "quote_asset": "USDT",
        "is_closed": true,
        "open_timestamp": 1623081868169,
        "closed_timestamp": 1623081868169,
        "avg_open_price": 77777,
        "avg_close_price": 100000,
        "min_open_price": 77777,
        "min_close_price": 100000,
        "max_open_price": 77777,
        "max_close_price": 100000,
        "size": 1.0,
        "is_long": true,
        "expiration_bar": false,
        "profit_taking_bar": true,
        "stop_loss_bar": false,
        "rule": false,
        "profit_taking_value": 100000,
        "stop_loss_value": 77771
    },
  ]
}
```

### Fund live trading stats (FUND)

```
GET /api/v1/hosted-strategy-live-trading-stats
```

**Weight:**
1

**Parameters:**

Name | Type | Mandatory | Values(default) | Description
------------ | ------------ | ------------ | ------------ | ------------
fund_id | int | YES |    | id from GET /api/v1/funds-info

**Data Source:**
Memory, Database

**Response:**


```javascript
{
  "fund-live-stats": {
      
       "USDT_BTC": 
            {   
                "timestamp": 1623081868169, 
                "active_subscribers": 1000,
                "aum_quote": 100,
                "aum_base": 100,
                "avg_returns_per_trade": 0.05,
                "best_won_quote": 0.10,
                "worst_loss_quote": -0.10,
                "best_won_base": 0.10,
                "worst_loss_base": 0.10,
                "total_traded_volume_quote": 100,
                "total_traded_volume_base": 100,
                "current_month_traded_volume_quote": 50,
                "current_month_traded_volume_base": 50,
                "current_month_pnl_quote": 2,
                "current_month_pnl_base": 1.5,
                "total_shared_profits": 100,
                "current_month_estiamted_shared_profits": 1000
            },
  }
}
```

### Fund paper trading stats (FUND)

```
GET /api/v1/hosted-strategy-paper-trading-stats
```

**Weight:**
1

**Parameters:**

Name | Type | Mandatory | Values(default) | Description
------------ | ------------ | ------------ | ------------ | ------------
fund_id | int | YES |    | id from GET /api/v1/funds-info

**Data Source:**
Memory, Database

**Response:**

```javascript
{
  "strategy-paper-stats": {
    "USDT_BTC": {
            "total_n_trades": 100, 
            "average_trade_per_week": 20,
            "avg_returns_per_trade": 0.05,
            "best_won_quote": 0.10,
            "worst_loss_quote": -0.10,
            "best_won_base": 0.10, 
            "worst_loss_base": 0.10,
            "total_traded_volume_quote": 100,
            "total_traded_volume_base": 100,
            "current_month_returns_quote": 0.1,
            "current_month_returns_base": 0.05
    }
  }
}
```
