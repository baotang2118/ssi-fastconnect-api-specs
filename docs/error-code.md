# FastConnect Error Codes (ErrCode)

Source: https://developers.ssi.com.vn/docs/api-reference/appendix/error-codes

## Overview

Every error response carries `status` (HTTP status code) and `ErrCode` (detailed error code).

## Naming convention

`ErrCode` follows `{HTTP_STATUS}{xxx}` where `xxx` is the detail code:

| Pattern | Range | Scope |
|---------|-------|-------|
| `4000xx` | 400000 – 400015 | Common to Trading and Market Data |
| `4001xx` | 400101 – 400165 | Trading only |
| `4002xx` | 400200+ | Market Data only |

## HTTP 200 — Success

| Code | Name | Description |
|------|------|-------------|
| 200 | Success | Request succeeded; server returned expected data. |

## HTTP 202 — Accepted

Request received and being processed asynchronously — not an error; the client must wait or retry.

| Code | Name | Description |
|------|------|-------------|
| 401114 | Push-approval is pending | Smart OTP push-approval waiting for user to approve in the app. SDKs (`ensure_authenticated` / `EnsureAuthenticated`) poll automatically; direct API callers must retry after a few seconds. |

## HTTP 400 — Bad Request

### Common errors (4000xx)

| Code | Name | Description |
|------|------|-------------|
| 400000 | Invalid Request | Request is invalid. |
| 400001 | Malformed JSON Body | JSON body has invalid syntax. |
| 400002 | Missing Required Field | A required field is missing. |
| 400003 | Invalid Field Value | Field value is invalid. |
| 400004 | Invalid Field Format | Wrong data format (regex/date/id). |
| 400005 | Invalid Field Error | Field is invalid. |
| 400006 | Missing Request Body | Request body missing. |
| 400007 | Data is available only for the latest x years | Data only supported for the last x years. |
| 400008 | Query time range cannot exceed y months | Query range exceeds y months. |
| 400009 | From Date cannot be greater than To Date | From Date must not exceed To Date. |
| 400010 | Query time range cannot exceed z years | Query range exceeds z years. |
| 400011 | The lookup time must be earlier than the current date | Lookup time must be in the past. |
| 400012 | Invalid Date/Timestamp | Invalid date/time format. |
| 400013 | Invalid Timezone | Invalid timezone. |
| 400014 | Invalid PageNumber | Invalid page number. |
| 400015 | Invalid PageSize | Invalid page size. |

### Trading — Authentication and configuration (400101 – 400110)

| Code | Name | Description |
|------|------|-------------|
| 400101 | Credential has no associated client | Credential not linked to any client. |
| 400102 | Failed to get 2FA info | Could not retrieve 2FA info. |
| 400103 | 2FA info not available for this account | 2FA info unavailable for this account. |
| 400104 | Failed to send OTP | OTP delivery failed. |
| 400105 | Unsupported Core_Type for account | Core_Type not supported for the account. |
| 400106 | Order rejected: stock disclosure policy violation | Order rejected for disclosure policy violation. |
| 400107 | Do not edit price and quantity at the same time | Cannot change price and quantity together. |
| 400108 | OrderType is not allowed | Order type not permitted. |
| 400109 | Modify order rejected: stock disclosure policy violation | Modify rejected for disclosure policy violation. |
| 400110 | No accounts found for client | No accounts found for the client. |

### Trading — Order business rules (400111 – 400148)

| Code | Name | Description |
|------|------|-------------|
| 400111 | Symbol Not Tradable | Symbol is not tradable. |
| 400112 | Price Out of Range | Price must be within ceiling/floor. |
| 400113 | Invalid Lot Size | Quantity must be divisible by the lot size. |
| 400114 | Price Must Be Positive | Price must be > 0. |
| 400115 | Price Must Be Zero | Price must be 0. |
| 400116 | Price Must Be Zero For Market Orders | Price must be 0 for ATO, ATC, MTL, MAK, MOK, PLO. |
| 400117 | Order Type Not Allowed In Current Session | Order type not allowed in the current session. |
| 400118 | Price Not Divisible By Tick Size | Price not divisible by tick size. |
| 400119 | Sell Not Allowed In This Session | Sell orders not allowed in this session. |
| 400120 | Quantity Below Minimum | Quantity below the minimum. |
| 400121 | Quantity Exceeds Maximum | Quantity above the maximum. |
| 400122 | Duplicate Order In Periodic Session | Duplicate order in a periodic session (both buy and sell). |
| 400123 | Symbol Not Registered With Exchange | Symbol not registered with any exchange. |
| 400124 | Symbol Rule Not Configured | No rule configured for the symbol. |
| 400125 | Symbol Even Lot Rule Not Configured | No even-lot rule configured for the symbol. |
| 400126 | Symbol Odd Lot Rule Not Configured | No odd-lot rule configured for the symbol. |
| 400127 | Cannot Change From Even To Odd Lot | Cannot modify quantity from even lot to odd lot. |
| 400128 | Cannot Change From Odd To Even Lot | Cannot modify quantity from odd lot to even lot. |
| 400129 | OrderQty Must Match Original | OrderQty must equal the quantity of the order being modified. |
| 400130 | Invalid Lot Quantity | Quantity larger than a lot and not divisible by the lot size. |
| 400131 | Order Cannot Be Cancelled | Order status does not allow cancellation. |
| 400132 | Order Cannot Be Modified | Order status does not allow modification. |
| 400133 | No Changes Detected | Modify request contains no changes. |
| 400134 | Invalid Symbol Format | Symbol must not contain spaces; max 20 characters. |
| 400135 | Pre-session Order Type Not Bound | Pre-session order type not bound to any session. |
| 400136 | Cannot Modify MP In Continuous Session | MP order cannot be modified in a continuous session. |
| 400137 | Cannot Modify MOK In Continuous Session | MOK order cannot be modified in a continuous session. |
| 400138 | Cannot Modify MTL In Continuous Session | MTL order cannot be modified in a continuous session. |
| 400139 | Cannot Modify MAK In Continuous Session | MAK order cannot be modified in a continuous session. |
| 400140 | Derivative Account Required | Order account must be a derivatives account. |
| 400141 | Symbol Buy Restricted | Symbol restricted for buying. |
| 400142 | Symbol Sell Restricted | Symbol restricted for selling. |
| 400143 | Symbol Not On Derivative Exchange | Symbol not on the derivatives exchange. |
| 400144 | Account Cannot Trade Derivative Symbol | Account cannot trade this derivatives symbol. |
| 400145 | Account Suspended | Account suspended for exceeding the allowed number of orders. |
| 400146 | Symbol Required | Symbol must not be empty. |
| 400147 | Quantity Must Be Positive | Quantity must be > 0. |
| 400148 | Quantity Exceeds Limit | Quantity exceeds the limit. |

### Trading — Sessions, accounts and FCO (400149 – 400165)

| Code | Name | Description |
|------|------|-------------|
| 400149 | Before Trading Hours | Before trading hours. |
| 400150 | After Trading Hours / Trading Account Required | After trading hours, or missing AccountNo/ClientId / account not found. |
| 400151 | Symbol Required | Missing `symbol`. |
| 400152 | Invalid Symbol | Invalid symbol. |
| 400153 | Order Side Required | Missing `side` (BUY/SELL). |
| 400154 | Invalid Order Side | Invalid side. |
| 400155 | Order Type Required | Missing `orderType`. |
| 400156 | Invalid Order Type | Invalid `orderType`. |
| 400157 | Quantity Required / Price Required | Missing quantity or price. |
| 400158 | Invalid Quantity | Invalid quantity. |
| 400159 | Invalid Price | Invalid price. |
| 400160 | Operation Not Permitted | Operation not permitted. |
| 400161 | FCO Strategy Not Supported | FCO strategy does not match the supported strategies. |
| 400162 | FCO Cannot Set StopOrder | This FCO type cannot set `stopOrder=True`. |
| 400163 | FCO Cannot Set Price Main Order | This FCO type cannot set a price for the main order. |
| 400164 | FCO Activation Limit Reached | Activation count is full; no more activations allowed. |
| 400165 | FCO Indicator Type Mismatch | Indicator values are not of the same data type. |

## HTTP 401 — Unauthorized

### Access token (401000 – 401016)

| Code | Name | Description |
|------|------|-------------|
| 401000 | Missing Authorization Header | Missing `Authorization` header. |
| 401001 | Unauthorized | Not authenticated. |
| 401002 | Access Token Missing | Access token missing. |
| 401003 | Access Token Invalid | Access token invalid. |
| 401004 | Access Token Expired | Access token expired. |
| 401009 | Access Token Not Yet Valid | Access token not yet valid (`nbf`). |
| 401010 | Token Signature Invalid | Token signature invalid. |
| 401011 | Missing Signature Header | Missing `X-Signature` header. |
| 401012 | Signature Not Recognized | Signature not recognized. |
| 401013 | Signature Not Valid | Signature invalid / token value invalid. |
| 401014 | Signature Not Matched | Signature mismatch. |
| 401015 | Signature Body Empty | Signature invalid: request body is empty. |
| 401016 | Token Not Include OTP | Token does not include OTP. |

### Refresh token and OTP (401100 – 401108)

| Code | Name | Description |
|------|------|-------------|
| 401100 | OTP Verification Failed | OTP verification failed. |
| 401101 | Invalid Refresh Token | Refresh token invalid. |
| 401102 | Refresh Token Missing Client | Refresh token missing client info. |
| 401103 | Refresh Token Revoked or Expired | Refresh token revoked or expired. |
| 401104 | Invalid API Key Reference | API key reference from refresh token is invalid. |
| 401105 | Invalid API Key or Secret | API key or secret invalid. |
| 401106 | Invalid Token Type | Wrong token type; refresh token required. |
| 401107 | Refresh Token Missing JTI | Refresh token missing `jti` claim. |
| 401108 | Refresh Token Missing Key ID | Refresh token missing `key_id` claim. |

## HTTP 403 — Forbidden

| Code | Name | Description |
|------|------|-------------|
| 403000 | Forbidden | No permission for the resource/operation. |
| 403001 | Insufficient Scope | Token/API key lacks the required scope. |
| 403100 | No Scopes Granted | API key has no scopes granted. |
| 403102 | Product Not Enabled For Account | Account has not enabled this product. |

## HTTP 404 — Not Found

| Code | Name | Description |
|------|------|-------------|
| 404000 | Resource Not Found | Requested resource not found. |
| 404100 | ClientRequestId Not Found | ClientRequestId not found. |
| 404101 | ClientRequestId Has No OrderId | No OrderId associated with the ClientRequestId. |
| 404102 | OrderId Not Found | OrderId not found. |
| 404103 | Account Not Found | Account not found. |
| 404204 | Symbol Not Found | Symbol not found. |

## HTTP 405 — Method Not Allowed

| Code | Name | Description |
|------|------|-------------|
| 405000 | Method Not Allowed | Method not supported for the endpoint. |
| 405001 | GET Not Allowed | Endpoint does not support GET. |
| 405002 | POST Not Allowed | Endpoint does not support POST. |
| 405003 | PUT Not Allowed | Endpoint does not support PUT. |
| 405004 | PATCH Not Allowed | Endpoint does not support PATCH. |
| 405005 | DELETE Not Allowed | Endpoint does not support DELETE. |
| 405006 | OPTIONS Not Allowed | Endpoint does not support OPTIONS. |

## HTTP 408 — Request Timeout

| Code | Name | Description |
|------|------|-------------|
| 408000 | Request Timeout | Request timed out. |
| 408200 | Market Data Export Request Timeout | Timeout receiving a market data export request. |

## HTTP 409 — Conflict

| Code | Name | Description |
|------|------|-------------|
| 409000 | Conflict | General state conflict. |
| 409100 | Duplicate Request | Duplicate request — already submitted with the same RequestId. |
| 409101 | Resource Already Exists | Resource already exists; cannot create. |
| 409102 | Insufficient Buying Power | Not enough buying power to place the order. |
| 409103 | Insufficient Account Balance | Account balance insufficient. |

## HTTP 429 — Too Many Requests

### General rate limits (429000 – 429100)

| Code | Name | Description |
|------|------|-------------|
| 429000 | Rate Limit Exceeded | System-wide request limit exceeded. |
| 429100 | Daily Order Limit Exceeded | Daily order limit for the account exceeded. |

### Trading limits (429101 – 429126)

| Code | Name | Description |
|------|------|-------------|
| 429101 | Volume Exceeds Current Room | Volume exceeds current room. |
| 429102 | Deposit Exceeds Approval Limit | Deposit exceeds the approval limit. |
| 429103 | Exceeds Allowable Input Limit | Exceeds the allowable input limit. |
| 429104 | Exceeds Allowable Approval Limit | Exceeds the allowable approval limit. |
| 429105 | Override Exceeds Withdrawable Amount | Override amount exceeds the withdrawable amount. |
| 429106 | Exceeds Withdrawable Amount | Exceeds the withdrawable amount. |
| 429107 | Exceeds Daily Transaction Limit | Exceeds the daily transaction limit. |
| 429108 | Exceeds Maximum Withdrawable Amount | Exceeds the maximum withdrawable amount. |
| 429109 | Exceeds Account Transaction Limit | Exceeds the account transaction amount. |
| 429110 | Exceeds Maximum Withdrawable Balance | Exceeds the maximum withdrawable balance. |
| 429111 | Exceeds Allowable Input Limit | Exceeds the allowable input limit. |
| 429112 | Exceeds Allowable Input Amount | Exceeds the allowable input amount. |
| 429113 | Exceeds Daily Transaction Limit | Exceeds the daily transaction limit. |
| 429114 | Exceeds Transferable Amount | Exceeds the transferable amount. |
| 429115 | Exceeds Maximum Transferable Amount | Exceeds the maximum transferable amount on approval. |
| 429116 | Exceeds Daily Transferable Limit | Exceeds the daily transferable limit. |
| 429117 | Transfer Exceeds Napas Limit | Transfer exceeds the Napas limit. |
| 429118 | Exceeds Maximum Withdrawable Balance | Exceeds the maximum withdrawable balance. |
| 429119 | Cash Advance Exceeds Maximum Limit | Cash advance exceeds the maximum allowed. |
| 429120 | Received Cash Advance Exceeds Maximum | Received cash advance exceeds the maximum allowed. |
| 429121 | Blocked Volume Exceeds Available Balance | Blocked volume exceeds the available balance. |
| 429122 | Released Volume Exceeds Blocked Volume | Released volume exceeds the blocked volume. |
| 429123 | Exceeds Allowable Input Limit | Exceeds the allowable input limit. |
| 429124 | Exceeds Auto-Approval Cash Advance | Exceeds the maximum auto-approved cash advance. |
| 429125 | Rejection Reason Length Exceeded | Rejection reason exceeds the allowed length. |
| 429126 | Volume Exceeds Limit | Volume exceeds the limit. |

## HTTP 500 — Internal Server Error

| Code | Name | Description |
|------|------|-------------|
| 500000 | Internal Server Error | Server-side error; request could not be processed. |
| 500002 | Database Execution Error | Business logic error at the database layer. |
| 500100 | Token Verification Failed: Client ID Mismatch | `client_id` mismatch. |
| 500101 | Token Verification Failed: Scopes Missing | Missing `scopes` claim. |
| 500200 | Market Data Persistence Error | Market data persistence error. |

## HTTP 502 — Bad Gateway

| Code | Name | Description |
|------|------|-------------|
| 502000 | Bad Gateway | General gateway error with an upstream system. |
| 502100 | CashPosition No Data | Upstream CashPosition returned no data for the account. |
| 502101 | GetCashBalance No Data | Upstream GetCashBalance returned no data for the account. |
| 502102 | GetAssetsExtends Failed | Upstream GetAssetsExtends failed for the account. |

## HTTP 503 — Service Unavailable

| Code | Name | Description |
|------|------|-------------|
| 503000 | Service Unavailable | Service temporarily unavailable. |
| 503001 | System Maintenance Window | System under maintenance. |

## HTTP 504 — Gateway Timeout

| Code | Name | Description |
|------|------|-------------|
| 504000 | Gateway Timeout | General timeout calling upstream. |
