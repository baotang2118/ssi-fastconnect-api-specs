# SSI FastConnect API Specs

Machine-readable API specifications and reference documentation for **SSI FastConnect** —
the trading and market data API provided by [SSI Securities Corporation](https://www.ssi.com.vn/)
(developer portal: <https://developers.ssi.com.vn/>).

## Documentation

- API reference: <https://developers.ssi.com.vn/docs/api-reference>
- Tutorial via code example (auth token): <https://developers.ssi.com.vn/docs/api-reference/auth-token>

## Repository contents

- [api.yaml](./api.yaml) — OpenAPI 3.1 spec for the FastConnect REST API (`https://api.ssi.com.vn`).
- [asyncapi.yaml](./asyncapi.yaml) — AsyncAPI 3.0 spec for the FastConnect WebSocket API
  (market/trading data stream at `wss://stream.ssi.com.vn` and the request/response
  trading WebSocket at `wss://api.ssi.com.vn/ws/v3/trading`).
- [docs/](./docs) — supplementary reference guides.

> **Why two spec formats?** `api.yaml` is the official **OpenAPI** spec for the REST endpoints.
> Plain OpenAPI has no support for streaming/WebSocket APIs, so `asyncapi.yaml` is written in
> **AsyncAPI** (modeled closely on the OpenAPI conventions used in `api.yaml`) to describe the
> FastConnect WebSocket channels and message schemas instead.

## Specification details

### REST API (`api.yaml`)

OpenAPI 3.1 definition, version `3.2.0`, covering:

| Area | Endpoints |
|------|-----------|
| Authentication | `auth/token`, `auth/requestOtp`, `auth/refresh` |
| Account | `account/info` |
| Trading | `trading/order`, `trading/position`, `trading/maxBuySell`, `trading/accountBalance`, `trading/ppmmrAccount`, `trading/orderBook` |
| Conditional orders (FCO) | `trading/fco/order`, `trading/fco/list`, `trading/fco/orderbook`, `trading/fco/statusHistory` |
| Market data | `data/securitiesByBoard`, `data/securitiesSummary`, `data/indexList`, `data/indexSummary`, `data/ohlc`, `data/masterdata` |

Authentication uses a bearer JWT (`securitySchemes.bearerAuth`). Signed trading
requests additionally require an `X-Signature` header.

### WebSocket API (`asyncapi.yaml`)

AsyncAPI 3.0 definition, version `3.2.0`, covering:

- **Market/trading stream** (`wss://stream.ssi.com.vn`) — subscribe/unsubscribe/list-subscription,
  ping/pong heartbeat, `TRADING` channel events (orders, FCO, portfolio) and `DATA` channel
  events (trade, quote, room, put, oddlot, market, market flag).
- **Trading WebSocket API** (`wss://api.ssi.com.vn/ws/v3/trading`, experimental) —
  request/response methods for `order.place` / `order.amend` / `order.cancel`,
  `fco.place` / `fco.cancel`, and unsigned query methods (`query.position`,
  `query.maxBuySell`, `query.accountBalance`, `query.ppmmrAccount`, `query.orderBook`).

### Reference docs (`docs/`)

- [docs/error-code.md](./docs/error-code.md) — `ErrCode` catalog and HTTP status conventions.
- [docs/fco-ref.md](./docs/fco-ref.md) — conditional order (FCO) types and their parameters.
- [docs/order-stt-flow.md](./docs/order-stt-flow.md) — order status lifecycle and state diagram.

## Usage

These specs can be loaded into any OpenAPI/AsyncAPI-compatible tool, for example:

```powershell
# Preview the REST API with Swagger UI / Redoc
npx @redocly/cli preview-docs api.yaml

# Preview the WebSocket API
npx @asyncapi/cli start studio asyncapi.yaml

# Validate both specs
npx @redocly/cli lint api.yaml
npx @asyncapi/cli validate asyncapi.yaml
```

They can also be used to generate client SDKs (e.g. via `openapi-generator` or
`@asyncapi/generator`) for the FastConnect REST and WebSocket APIs.

## License

This project is licensed under the [MIT License](./LICENSE).
