# REST Authenticated Endpoints

## Wallet Balances

> Example request

```curl
curl -X POST "https://durex/v1/auth/r/balances?user_id=a"
```

See your balances

### HTTP REQUEST

`POST /v1/auth/r/balances`

> Example Response

```json
[
  [
    UserId, 
    // WALLET_TYPE,
    Asset,  // CURRENCY,
    // BALANCE,
    OpId,
    OpType,
    OpTime
    Change,
    FreezeBalance,  // UNSETTLED_INTEREST,
    AvailableBalance  // BALANCE_AVAILABLE
  ],
  ["a","BTC",9,"bidMakerFreezeStock",1534228785,"-5.00000000","1.00000000","978.00000000"],
  ["a","USD",9,"bidMakerMoney",1534228785,"175.00000000","0.00000000","400.00000000"],
  ...
]
```

## Withdrawal

> Example request

```curl
curl -X POST "https://durex/v1/auth/w/balances/USD/withdraw?user_id=b&amount=1000"
```

> Example Response

```json
[
  OpId  //WITHDRAWAL_ID
]

[2]
```

Allow you to request a withdrawal from one of your wallet.

### HTTP REQUEST

`POST /v1/auth/w/balances/<Asset>/withdraw`

### ARGUMENTS

 Parameter | Type | Required | Description
---------- | ---- | -------- | ------------
 Asset | string | Required | Currency (USD, etc)
 amount | string | Required | Amount to withdraw.

## New Order

> Example request

```curl
curl -X POST "https://durex/v1/auth/w/order/new?type=1&symbol=tBTCUSD&user_id=a&side=1&price=10&amount=10&taker_fee=0&maker_fee=0"

curl -X POST "https://durex/v1/auth/w/order/new?type=2&symbol=tBTCUSD&user_id=a&side=1&amount=10&taker_fee=0"
```

> Example Response

```json
[
  OrderId  // ID
]

[3]
```

Submit a new Order

### HTTP REQUEST

`POST /v1/auth/w/order/new`

### ARGUMENTS

 Parameter | Type | Required | Description
---------- | ---- | -------- | ------------
 type | string | Required | Either 1(market) or 2(limit)
 symbol | string | Required | The name of the symbol (see /symbols).
 side | int | Required | Either 1(sell) or 2(buy).
 price | float | Required | Price to buy or sell at. Must be positive. No need for market orders.
 amount | float | Required | Order size: how much you want to buy or sell
 taker_fee | float | Required | Taker fee rate (0 <= taker_fee <= 1)
 maker_fee | float | Required | Maker fee rate (0 <= taker_fee <= 1)

## Cancel Order

> Example request

```curl
curl -X POST "https://durex/v1/order/cancel?symbol=tBTCUSD&user_id=a&order_id=3"
```

> Example Response

```json
[
  OpId
]

[4]
```

Cancel an order.

### HTTP REQUEST

`POST /v1/auth/w/order/cancel`

### ARGUMENTS

 Parameter | Type | Required | Description
---------- | ---- | -------- | ------------
 symbol | string | Required | Symbol (tBTCUSD, ...)
 order_id | int64 | Required | The order ID given by /order/new.

## Active Order Status

> Example request

```curl
curl -X POST "https://durex/v1/auth/r/order/status?symbol=tBTCUSD&user_id=b&order_id=9"
```

> Example Response

```json
[
  OrderId,  // ID,
  Market,  // SYMBOL,
  OrderType,  // TYPE,
  Side,
  UserId,
  CreateTime,  // MTS_CREATE,
  UpdateTime,  // MTS_UPDATE,
  Price,  // PRICE,
  Amount,  // AMOUNT,
  // AMOUNT_ORIG,
  // ORDER_STATUS,
  // PRICE_AVG
  TakerFee,
  MakerFee,
  Left,
  Freeze,
  DealStock,
  DealMoney,
  DealFee
]

[9,"tBTCUSD",1,2,"b",1534228785,1534228785,"55.00000000","8.00000000","0.00000000","0.00000000","3.00000000","165.00000000","5.00000000","175.00000000","0.00000000"]
```

Get the status of an active order.

### HTTP REQUEST

`POST /v1/auth/r/order/status`

### ARGUMENTS

 Parameter | Type | Required | Description
---------- | ---- | -------- | ------------
 symbol | string | Required | Symbol (tBTCUSD, ...)
 order_id | int64 | Required | The order ID given by /order/new.

## Past Order Status

> Example request

```curl
curl -X POST "https://durex/v1/auth/r/order/status/hist?symbol=tBTCUSD&user_id=b&order_id=9"
```

> Example Response

```json
[
  OrderId,  // ID,
  Market,  // SYMBOL,
  OrderType,  // TYPE,
  Side,
  UserId,
  CreateTime,  // MTS_CREATE,
  FinishTime,  // MTS_UPDATE,
  Price,  // PRICE,
  Amount,  // AMOUNT,
  // AMOUNT_ORIG,
  // ORDER_STATUS,
  // PRICE_AVG
  TakerFee,
  MakerFee,
  // Left,
  // Freeze,
  DealStock,
  DealMoney,
  DealFee
]

[9,"tBTCUSD",1,2,"b",1534228785,1534228785,"55.00000000","8.00000000","0.00000000","0.00000000","5.00000000","175.00000000","0.00000000"]
```

Get the status of an closed or canceled order.

### HTTP REQUEST

`POST /v1/auth/r/order/status/hist`

### ARGUMENTS

 Parameter | Type | Required | Description
---------- | ---- | -------- | ------------
 symbol | string | Required | Symbol (tBTCUSD, ...)
 order_id | int64 | Required | The order ID given by /order/new.

## Active Orders

> Example request

```curl
curl -X POST "https://durex/v1/auth/r/orders/tBTCUSD?user_id=b"
```

> Example Response

```json
[
  [
    OrderId,  // ID,
    Market,  // SYMBOL,
    OrderType,  // TYPE,
    Side,
    UserId,
    CreateTime,  // MTS_CREATE,
    UpdateTime,  // MTS_UPDATE,
    Price,  // PRICE,
    Amount,  // AMOUNT,
    // AMOUNT_ORIG,
    // ORDER_STATUS,
    // PRICE_AVG
    TakerFee,
    MakerFee,
    Left,
    Freeze,
    DealStock,
    DealMoney,
    DealFee
  ],
  [9,"tBTCUSD",1,2,"b",1534228785,1534228785,"55.00000000","8.00000000","0.00000000","0.00000000","3.00000000","165.00000000","5.00000000","175.00000000","0.00000000"],
  ...
]
```

View your active orders.

### HTTP REQUEST

`POST /v1/auth/r/orders/<Symbol>`

### ARGUMENTS

 Parameter | Type | Required | Description
---------- | ---- | -------- | ------------
 Symbol | string | Required | Symbol (tBTCUSD, ...)

## Past Orders

> Example request

```curl
curl -X POST "https://durex/v1/auth/r/orders/tBTCUSD/hist?user_id=b&start=1534228780&end=1534238780&limit=2&offset=1"
```

> Example Response

```json
[
  [
    OrderId,  // ID,
    Market,  // SYMBOL,
    OrderType,  // TYPE,
    Side,
    UserId,
    CreateTime,  // MTS_CREATE,
    FinishTime,  // MTS_UPDATE,
    Price,  // PRICE,
    Amount,  // AMOUNT,
    // AMOUNT_ORIG,
    // ORDER_STATUS,
    // PRICE_AVG
    TakerFee,
    MakerFee,
    // Left,
    // Freeze,
    DealStock,
    DealMoney,
    DealFee
  ],
  [9,"tBTCUSD",1,2,"b",1534228785,1534228785,"55.00000000","8.00000000","0.00000000","0.00000000","5.00000000","175.00000000","0.00000000"],
  ...
]
```

Returns the most recent closed or canceled orders up to circa two weeks ago

### HTTP REQUEST

`POST /v1/auth/r/orders/<Symbol>/hist`

### ARGUMENTS

 Parameter | Type | Required | Description
---------- | ---- | -------- | ------------
 Symbol | string | Required | Symbol (tBTCUSD, ...)
 start | int64 | Optional | Start timestamp
 end | int64 | Optional | End timestamp
 limit | uint64 | Optional | Number of records (0: all)
 offset | uint64 | Optional | Offset position

## Past Trades

> Example request

```curl
curl -X POST "https://durex/v1/auth/r/trades/tBTCUSD/hist?user_id=b&start=1534228780&end=1534238780&limit=2&offset=1"
```

> Example Response

```json
[
  [
    OrderId,  // ORDER_ID,
    DealOrderId,  // MAKER
    Market,  // PAIR,
    DealRole,
    Side,
    UserId,
    Time,  // MTS_CREATE
    Price,  // EXEC_PRICE
    Amount,
    Deal,  // EXEC_AMOUNT
    // ORDER_TYPE,
    Fee,  // FEE
    DealFee
  ],
  [8,5,"tBTCUSD",1,2,"b",1534230305,"25.00000000","1.00000000","25.00000000","0.00000000","0.00000000"],
  ...
]
```

View your past trades.

### HTTP REQUEST

`POST /v1/auth/r/trades/<Symbol>/hist`

### ARGUMENTS

 Parameter | Type | Required | Description
---------- | ---- | -------- | ------------
 Symbol | string | Required | Symbol (tBTCUSD, ...)
 start | int64 | Optional | Start timestamp
 end | int64 | Optional | End timestamp
 limit | uint64 | Optional | Number of records (0: all)
 offset | uint64 | Optional | Offset position