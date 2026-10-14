# Conditional Order (FCO) Reference

Source: https://developers.ssi.com.vn/docs/api-reference/appendix/fco-reference

## Overview

FCO (FastConnect Conditional Order) lets you place orders that fire automatically when
preset conditions are met. Seven order types are supported, each with its own extra parameters.

All conditional orders are sent to `POST /api/v3/trading/fco/order`; the `type` field selects
the order type.

## Common parameters

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `accountNo` | string | Yes | Trading account (equity or derivatives). |
| `symbol` | string | Yes | Security symbol or derivatives contract code. |
| `side` | string | Yes | `B` (Buy) or `S` (Sell). |
| `type` | string | Yes | Conditional order type (see table below). |
| `quantity` | integer | Yes | Order quantity. |
| `deviceId` | string | Yes | Device identifier (MAC address or IMEI). |
| `price` | string | No | Order price. Accepts a numeric price or a special type (`MP`, `MOK`, `MTL`). |
| `priceSlip` | number | No | Allowed price slippage. |
| `from` | string | No | Validity start, format `YYYY/MM/DD hh:mm:ss`. Defaults to order creation time. |
| `to` | string | No | Validity end, format `YYYY/MM/DD hh:mm:ss`. Default 24h, max 31 days. |
| `code` | string | No | OTP or PIN code (required if the token was not issued with OTP). |
| `userAgent` | string | No | Browser/app info of the caller. |
| `clientRequestId` | string | No | Client-generated id used to correlate response and streaming events. |
| `ipAddress` | string | No | Device IP address. |

## Order types

### 1. Stop Limit (`stop_limit`)

Like a Stop order, but the order is sent to the exchange at the client's limit price.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `price` | string | Yes | Limit price used when pushing to the exchange. |
| `stopPrice` | number | Yes | Stop price. The order triggers when the market price reaches this level. |
| `operator` | string | Yes | Comparison operator: `greater_or_equal`, `lesser_or_equal`, `greater`, `lesser`, `equal`. |

```json
{
  "symbol": "SSI",
  "side": "S",
  "type": "stop_limit",
  "price": "30500",
  "priceSlip": 0,
  "quantity": 1000,
  "accountNo": "0001234",
  "from": "2025/07/10 08:00:00",
  "to": "2025/07/15 23:59:59",
  "stopPrice": 30500,
  "operator": "greater_or_equal",
  "code": "",
  "userAgent": "agent fctrading app",
  "deviceId": "0A:B1:92:D3:5S:8G"
}
```

### 2. Stop (`stop`)

Stop order. Triggers when the price condition is met; sent to the exchange at market price.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `stopPrice` | number | Yes | Stop price that triggers the order. |
| `operator` | string | Yes | Comparison operator (see list above). |

```json
{
  "symbol": "SSI",
  "side": "S",
  "type": "stop",
  "price": "30500",
  "priceSlip": 0,
  "quantity": 1000,
  "accountNo": "0001234",
  "from": "2025/07/10 08:00:00",
  "to": "2025/07/15 23:59:59",
  "stopPrice": 30500,
  "operator": "greater_or_equal",
  "code": "",
  "userAgent": "agent fctrading app",
  "deviceId": "0A:B1:92:D3:5S:8G"
}
```

### 3. Good Till Date (`gtd`)

Multi-day order. Triggered every day until one of these happens:

- the original quantity is fully matched
- the user cancels the order
- the validity end date passes

No type-specific parameters, but `from` and `to` should be set to define the validity window.

```json
{
  "symbol": "SSI",
  "side": "S",
  "type": "gtd",
  "price": "30500",
  "priceSlip": 0,
  "quantity": 1000,
  "accountNo": "0001234",
  "from": "2025/07/10 08:00:00",
  "to": "2025/07/15 23:59:59",
  "code": "",
  "userAgent": "agent fctrading app",
  "deviceId": "0A:B1:92:D3:5S:8G"
}
```

### 4. Bull Bear (`bullbear`)

Take-profit / stop-loss order when **no position exists yet**: attaches a TP and an SL to a main order.
When the main order matches, the system generates an OCO order on the opposite side.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `tpActivePrice` | number | Yes | Take-profit activation price. |
| `tpPrice` | string | Yes | Take-profit price. Accepts numeric or `MP`, `MOK`, `MTL`. |
| `tpSlip` | number | Yes | Take-profit slippage. |
| `slActivePrice` | number | Yes | Stop-loss activation price. |
| `slPrice` | string | Yes | Stop-loss price. Accepts numeric or `MP`, `MOK`, `MTL`. |
| `slSlip` | number | Yes | Stop-loss slippage. |

Derivatives example:

```json
{
  "symbol": "41I1F7000",
  "side": "B",
  "type": "bullbear",
  "price": "MTL",
  "priceSlip": 0.50,
  "quantity": 1,
  "accountNo": "0D01234",
  "from": "2025/07/10 08:00:00",
  "to": "2025/07/15 23:59:59",
  "stopPrice": 0,
  "activePrice": 0.0,
  "trailingAmount": 0,
  "tpActivePrice": 1555,
  "slActivePrice": 1540,
  "tpPrice": "1556",
  "slPrice": "1541",
  "tpSlip": 1,
  "slSlip": 1,
  "operator": "",
  "code": "",
  "userAgent": "agent fctrading app",
  "deviceId": "0A:B1:92:D3:5S:8G"
}
```

### 5. OCO — One Cancels Other (`oco`)

Paired take-profit / stop-loss for an **existing position** in the portfolio.
When one leg (a) triggers, the system checks the other leg (b): if (b) was already activated its
child order is cancelled first, then the triggered order is sent with quantity equal to the
remaining unmatched quantity of (b)'s child order.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `tpActivePrice` | number | Yes | Take-profit activation price. |
| `tpPrice` | string | Yes | Take-profit price. Accepts numeric or `MP`, `MTL`, `MOK`, `MAK`. |
| `tpSlip` | number | Yes | Take-profit slippage. |
| `slActivePrice` | number | Yes | Stop-loss activation price. |
| `slPrice` | string | Yes | Stop-loss price. Accepts numeric or `MP`, `MTL`, `MOK`, `MAK`. |
| `slSlip` | number | Yes | Stop-loss slippage. |

```json
{
  "symbol": "SSI",
  "side": "S",
  "type": "oco",
  "price": "MP",
  "priceSlip": 0.0,
  "quantity": 1000,
  "accountNo": "0001234",
  "from": "2025/07/10 08:00:00",
  "to": "2025/07/15 23:59:59",
  "stopPrice": 0,
  "activePrice": 0.0,
  "trailingAmount": 0,
  "tpActivePrice": 30500,
  "slActivePrice": 20000,
  "tpPrice": "30500",
  "slPrice": "30500",
  "tpSlip": 100,
  "slSlip": 100,
  "operator": "",
  "code": "",
  "userAgent": "agent fctrading app",
  "deviceId": "0A:B1:92:D3:5S:8G"
}
```

### 6. Trailing Stop (`trailing_stop`)

Stop order whose stop price follows the market trend. Sent to the exchange at market price.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `activePrice` | number | Yes | Initial activation price of the conditional order. |
| `trailingAmount` | number | Yes | Trailing distance between market price and stop price. |

```json
{
  "symbol": "SSI",
  "side": "S",
  "type": "trailing_stop",
  "price": "MP",
  "priceSlip": 0.0,
  "quantity": 1000,
  "accountNo": "0001234",
  "from": "2025/07/10 08:00:00",
  "to": "2025/07/15 23:59:59",
  "activePrice": 28000,
  "trailingAmount": 1000,
  "code": "",
  "userAgent": "agent fctrading app",
  "deviceId": "0A:B1:92:D3:5S:8G"
}
```

### 7. Trailing Stop Limit (`trailing_stop_limit`)

Stop order whose stop price follows the market trend; the order is sent at a limit price
computed from the stop price.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `price` | string | Yes | Limit price used when pushing to the exchange. |
| `activePrice` | number | Yes | Initial activation price. |
| `trailingAmount` | number | Yes | Trailing distance. |

```json
{
  "symbol": "SSI",
  "side": "S",
  "type": "trailing_stop_limit",
  "price": "MP",
  "priceSlip": 0.0,
  "quantity": 1000,
  "accountNo": "0001234",
  "from": "2025/07/10 08:00:00",
  "to": "2025/07/15 23:59:59",
  "activePrice": 28000,
  "trailingAmount": 1000,
  "userAgent": "agent fctrading app",
  "deviceId": "0A:B1:92:D3:5S:8G"
}
```

## Summary of types

| # | Name | `type` | Type-specific fields |
|---|------|--------|----------------------|
| 1 | Stop Limit | `stop_limit` | `price`, `stopPrice`, `operator` |
| 2 | Stop | `stop` | `stopPrice`, `operator` |
| 3 | Good Till Date | `gtd` | (none) |
| 4 | Bull Bear | `bullbear` | `tpActivePrice`, `tpPrice`, `tpSlip`, `slActivePrice`, `slPrice`, `slSlip` |
| 5 | OCO | `oco` | `tpActivePrice`, `tpPrice`, `tpSlip`, `slActivePrice`, `slPrice`, `slSlip` |
| 6 | Trailing Stop | `trailing_stop` | `activePrice`, `trailingAmount` |
| 7 | Trailing Stop Limit | `trailing_stop_limit` | `price`, `activePrice`, `trailingAmount` |

## `operator` values

Used by `stop` and `stop_limit` to define the trigger condition:

| Value | Condition |
|-------|-----------|
| `greater_or_equal` | market price ≥ stop price |
| `lesser_or_equal` | market price ≤ stop price |
| `greater` | market price > stop price |
| `lesser` | market price < stop price |
| `equal` | market price = stop price |

## Special price types

| Value | Meaning |
|-------|---------|
| `MP` | Market Price |
| `MOK` | Match or Kill |
| `MTL` | Match then Limit |
| `MAK` | Match and Kill (cancel remainder) |
