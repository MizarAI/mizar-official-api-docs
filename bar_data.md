# Exchange data API 

### Get exchange information
```
GET /v1/exchanges
```
**Weight:**
1

**Parameters:**
NONE

### Available quote assets per exchange
```
GET /v1/quote-assets
```
**Weight:**
1

**Parameters:**

Name | Type | Mandatory | Values(default) | Description
------------ | ------------ | ------------ | ------------ | ------------
exchange | string | YES |   | exchange from GET /v1/exchanges

### Available base assets per exchange
```
GET /v1/base-assets
```
**Weight:**
1

**Parameters:**

Name | Type | Mandatory | Values(default) | Description
------------ | ------------ | ------------ | ------------ | ------------
exchange | string | YES |   | exchange from GET /v1/exchanges

### Available base assets per exchange
```
GET /v1/base-assets
```
**Weight:**
1

**Parameters:**

Name | Type | Mandatory | Values(default) | Description
------------ | ------------ | ------------ | ------------ | ------------
exchange | string | YES |   | exchange from GET /v1/exchanges


### Available bar types
```
GET /v1/bar-types
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


### Bar data
```
GET /v1/bar
```
**Weight:**
1

**Parameters:**

Name | Type | Mandatory | Values(default) | Description
------------ | ------------ | ------------ | ------------ | ------------
base_asset | str | YES |    | base_asset from GET /v1/base-assets
quote_asset | str | YES |    | quote_asset from GET /v1/quote-assets
start_timestamp | int | YES |    | timestamp in seconds
limit | int | YES |    | max 500
bar_type | str | YES |    | bar_type from GET /v1/bar-types
bar_subclass | str | YES |    | bar_subclass from GET /v1/bar-types
exchange | string | YES |   | exchange from GET /v1/exchanges
