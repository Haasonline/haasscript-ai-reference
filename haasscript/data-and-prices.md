# Data & Prices

62 commands across 5 categories.

## Categories

- [Price Data](#price-data) (16 commands)
- [Price Market Information](#price-market-information) (12 commands)
- [Profit Information](#profit-information) (6 commands)
- [Time Information](#time-information) (18 commands)
- [Wallet](#wallet) (10 commands)

---

# Price Data

## BuyPrices

```lua
BuyPrices([interval], [fullCandles], [market], [hlcStyle])
```

**Aliases:** `AskPrices`

Gets the buy/ask prices.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `interval` | `number` | No | The ticks interval. |
| `fullCandles` | `boolean` | No | If true, the last closed candles will be returned. |
| `market` | `string` | No | The market of the tick. Default is the selected main market. |
| `hlcStyle` | `boolean` | No | When enabled, the data returned will be adjusted for HLC instead of OHLC. Meaning that the OHL data can change. |

**Returns:** `HaasNumberCollection`

Returns the buy/ask prices.

---

## ClosePrices

```lua
ClosePrices([interval], [fullCandles], [market], [hlcStyle])
```

**Aliases:** `Prices`

A collection with close prices. Sorted from new to old.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `interval` | `number` | No | The ticks interval. |
| `fullCandles` | `boolean` | No | If true, the last closed candles will be returned. |
| `market` | `string` | No | The market of the tick. Default is the selected main market. |
| `hlcStyle` | `boolean` | No | When enabled, the data returned will be adjusted for HLC instead of OHLC. Meaning that the OHL data can change. |

**Returns:** `HaasNumberCollection`

Returns a collection of close prices.

---

## CurrentPrice

```lua
CurrentPrice([market])
```

Gets all the price data from the current candle.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The market of the tick. Default is the selected main market. |

**Returns:** `dynamic`

Returns the price data in an array.

---

## GetBodyHighPrices

```lua
GetBodyHighPrices([interval], [fullCandles], [market], [hlcStyle])
```

A collection with the highest price of the candle its body prices. Sorted from new to old.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `interval` | `number` | No | The ticks interval. |
| `fullCandles` | `boolean` | No | If true, the last closed candles will be returned. |
| `market` | `string` | No | The market of the tick. Default is the selected main market. |
| `hlcStyle` | `boolean` | No | When enabled, the data returned will be adjusted for HLC instead of OHLC. Meaning that the OHL data can change. |

**Returns:** `HaasNumberCollection`

Returns a collection of the highest price of the candle its body prices.

---

## GetBodyLowPrices

```lua
GetBodyLowPrices([interval], [fullCandles], [market], [hlcStyle])
```

A collection with the lowest price of the candle its body prices. Sorted from new to old.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `interval` | `number` | No | The ticks interval. |
| `fullCandles` | `boolean` | No | If true, the last closed candles will be returned. |
| `market` | `string` | No | The market of the tick. Default is the selected main market. |
| `hlcStyle` | `boolean` | No | When enabled, the data returned will be adjusted for HLC instead of OHLC. Meaning that the OHL data can change. |

**Returns:** `HaasNumberCollection`

Returns a collection of the lowest price of the candle its body prices.

---

## GetVolume

```lua
GetVolume([interval], [fullCandles], [market], [hlcStyle])
```

A collection with volume data. Sorted from new to old.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `interval` | `number` | No | The ticks interval. |
| `fullCandles` | `boolean` | No | If true, the last closed candles will be returned. |
| `market` | `string` | No | The market of the tick. Default is the selected main market. |
| `hlcStyle` | `boolean` | No | When enabled, the data returned will be adjusted for HLC instead of OHLC. Meaning that the OHL data can change. |

**Returns:** `HaasNumberCollection`

Returns a collection with volume data.

---

## HeikinOpenPrices

```lua
HeikinOpenPrices([interval], [fullCandles], [market])
```

Gets the Heikin-Ashi open prices.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `interval` | `number` | No | The ticks interval. |
| `fullCandles` | `boolean` | No | If true, the last closed candles will be returned. |
| `market` | `string` | No | The market of the tick. Default is the selected main market. |

**Returns:** `HaasNumberCollection`

Returns the Heikin-Ashi open prices.

---

## HighPrices

```lua
HighPrices([interval], [fullCandles], [market], [hlcStyle])
```

A collection with high prices. Sorted from new to old.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `interval` | `number` | No | The ticks interval. |
| `fullCandles` | `boolean` | No | If true, the last closed candles will be returned. |
| `market` | `string` | No | The market of the tick. Default is the selected main market. |
| `hlcStyle` | `boolean` | No | When enabled, the data returned will be adjusted for HLC instead of OHLC. Meaning that the OHL data can change. |

**Returns:** `HaasNumberCollection`

Returns a collection of high prices.

---

## HLCPrices

```lua
HLCPrices([interval], [fullCandles], [market], [hlcStyle])
```

A collection with the average of high, low and close prices. Sorted from new to old.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `interval` | `number` | No | The ticks interval. |
| `fullCandles` | `boolean` | No | If true, the last closed candles will be returned. |
| `market` | `string` | No | The market of the tick. Default is the selected main market. |
| `hlcStyle` | `boolean` | No | When enabled, the data returned will be adjusted for HLC instead of OHLC. Meaning that the OHL data can change. |

**Returns:** `HaasNumberCollection`

Returns a collection of the average of high, low and close prices.

---

## HLPrices

```lua
HLPrices([interval], [fullCandles], [market], [hlcStyle])
```

A collection with an average of high and low prices. Sorted from new to old.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `interval` | `number` | No | The ticks interval. |
| `fullCandles` | `boolean` | No | If true, the last closed candles will be returned. |
| `market` | `string` | No | The market of the tick. Default is the selected main market. |
| `hlcStyle` | `boolean` | No | When enabled, the data returned will be adjusted for HLC instead of OHLC. Meaning that the OHL data can change. |

**Returns:** `HaasNumberCollection`

Returns a collection of an average of high and low prices.

---

## LowPrices

```lua
LowPrices([interval], [fullCandles], [market], [hlcStyle])
```

A collection with low prices. Sorted from new to old.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `interval` | `number` | No | The ticks interval. |
| `fullCandles` | `boolean` | No | If true, the last closed candles will be returned. |
| `market` | `string` | No | The market of the tick. Default is the selected main market. |
| `hlcStyle` | `boolean` | No | When enabled, the data returned will be adjusted for HLC instead of OHLC. Meaning that the OHL data can change. |

**Returns:** `HaasNumberCollection`

Returns a collection of low prices.

---

## OCPrices

```lua
OCPrices([interval], [fullCandles], [market], [hlcStyle])
```

A collection with an average of open and close prices. Sorted from new to old.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `interval` | `number` | No | The ticks interval. |
| `fullCandles` | `boolean` | No | If true, the last closed candles will be returned. |
| `market` | `string` | No | The market of the tick. Default is the selected main market. |
| `hlcStyle` | `boolean` | No | When enabled, the data returned will be adjusted for HLC instead of OHLC. Meaning that the OHL data can change. |

**Returns:** `HaasNumberCollection`

Returns a collection of an average of open and close prices.

---

## OHLCPrices

```lua
OHLCPrices([interval], [fullCandles], [market], [hlcStyle])
```

**Aliases:** `HeikenClosePrices`

A collection with the average of open, high, low and close prices. Sorted from new to old.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `interval` | `number` | No | The ticks interval. |
| `fullCandles` | `boolean` | No | If true, the last closed candles will be returned. |
| `market` | `string` | No | The market of the tick. Default is the selected main market. |
| `hlcStyle` | `boolean` | No | When enabled, the data returned will be adjusted for HLC instead of OHLC. Meaning that the OHL data can change. |

**Returns:** `HaasNumberCollection`

Returns a collection of the average of open, high, low and close prices.

---

## OpenPrices

```lua
OpenPrices([interval], [fullCandles], [market], [hlcStyle])
```

A collection with open prices. Sorted from new to old.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `interval` | `number` | No | The ticks interval. |
| `fullCandles` | `boolean` | No | If true, the last closed candles will be returned. |
| `market` | `string` | No | The market of the tick. Default is the selected main market. |
| `hlcStyle` | `boolean` | No | When enabled, the data returned will be adjusted for HLC instead of OHLC. Meaning that the OHL data can change. |

**Returns:** `HaasNumberCollection`

Returns a collection of open prices.

---

## SellPrices

```lua
SellPrices([interval], [fullCandles], [market], [hlcStyle])
```

**Aliases:** `BidPrices`

A collection with sell/ask prices. Sorted from new to old.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `interval` | `number` | No | The ticks interval. |
| `fullCandles` | `boolean` | No | If true, the last closed candles will be returned. |
| `market` | `string` | No | The market of the tick. Default is the selected main market. |
| `hlcStyle` | `boolean` | No | When enabled, the data returned will be adjusted for HLC instead of OHLC. Meaning that the OHL data can change. |

**Returns:** `HaasNumberCollection`

Returns a collection of sell/ask prices.

---

## SourcePrices

```lua
SourcePrices(sourceType, [interval], [fullCandles], [market], [hlcStyle])
```

Gets the price source based on the input enum given. Can be any of Enumerations Source Price Types.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `sourceType` | `enum` | Yes | Source price type enum. |
| `interval` | `number` | No | The ticks interval. |
| `fullCandles` | `boolean` | No | If true, the last closed candles will be returned. |
| `market` | `string` | No | The market of the tick. Default is the selected main market. |
| `hlcStyle` | `boolean` | No | When enabled, the data returned will be adjusted for HLC instead of OHLC. Meaning that the OHL data can change. |

**Returns:** `HaasNumberCollection`

Returns a collection of prices.

---


# Price Market Information

## AverageCandleSize

```lua
AverageCandleSize([market])
```

Calculates the average candle body size of the last 50 minutes.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |

**Returns:** `number`

Returns the average candle body size.

---

## AverageOrderbookSpread

```lua
AverageOrderbookSpread([market])
```

Calculates the average orderbook bid/ask spread of the last 50 minutes.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |

**Returns:** `number`

Returns the average orderbook bid/ask spread.

---

## CreateMarket

```lua
CreateMarket([priceSource], [baseCurrency], [quoteCurrency], [contractName])
```

Creates a market string that can be used for a variety of commands.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `priceSource` | `string` | No | The price source of the market. If empty the main bot price source is set. |
| `baseCurrency` | `string` | No | The base currency of the market. If empty the bots base currency price source is set. |
| `quoteCurrency` | `string` | No | The quote currency of the market. If empty the bots main quote currency is set. |
| `contractName` | `string` | No | The contract name of the market. If empty the bots main contract name is set. |

**Returns:** `string`

Returns the market string.

---

## GetLastTrades

```lua
GetLastTrades([depth], [market])
```

Gets a list of the last trades.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `depth` | `number` | No | Maximum number of records returned (can return less). |
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |

**Returns:** `HaasNumberCollection`

Returns the last trades. Array format: [[price, amount, isBuy?, unix], ... ].

---

## GetOrderbook

```lua
GetOrderbook([market])
```

Gets the orderbook on a specific market.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |

**Returns:** `HaasNumberCollection`

Returns a collection with 4 arrays.

---

## GetOrderbookAsk

```lua
GetOrderbookAsk([market])
```

Gets a list of the orderbook ask records.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |

**Returns:** `dynamic`

Returns an array of bid prices and order sizes. Output format: [[price, amount], [price, amount] ...].

---

## GetOrderbookBid

```lua
GetOrderbookBid([market])
```

Gets a list of the orderbook bid records.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |

**Returns:** `dynamic`

Returns an array of bid prices and order sizes. Output format: [[price, amount], [price, amount] ...].

---

## LastBuyTradesCommand

```lua
LastBuyTradesCommand([Seconds back], [market])
```

Returns all last selling orders

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `Seconds back` | `number` | No |  |
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |

**Returns:** `number`

Returns the buying trading volume

---

## LastSellTradesCommand

```lua
LastSellTradesCommand([Seconds back], [market])
```

Returns all last selling orders

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `Seconds back` | `number` | No |  |
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |

**Returns:** `number`

Returns the selling trading volume

---

## LastTradesSentiment

```lua
LastTradesSentiment([market])
```

Calculates the last trades sentiment as a number.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |

**Returns:** `number`

Returns the last trades sentiment. 0.5 is the default, idle value.

---

## OrderbookSentiment

```lua
OrderbookSentiment([market])
```

Calculates the orderbook sentiment as a number.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |

**Returns:** `number`

Returns the orderbook sentiment. 0.5 is the default, idle value.

---

## PriceLevel

```lua
PriceLevel(price)
```

Determines the price level. For example. 123456 will return 10000. 123 will return 100.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `price` | `number` | Yes | Source data. |

**Returns:** `number`

Returns the price level.

---


# Profit Information

## GetBotProfit

```lua
GetBotProfit([market], [includeUnrealized])
```

Total realized profits on a specific market. Unrealized profits are included when 'includeUnrealized' is set on true.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The guid returned by AccountGuid(), InputAccount or InputAccountMarket for example. |
| `includeUnrealized` | `boolean` | No | Set on true to include the unrealized profits. By default false. |

**Returns:** `number`

Return total realized profit. On spot/margin the result will be in the quote currency. For leverage its in the underlying currency.

---

## GetBotROI

```lua
GetBotROI([market])
```

Gets the return on investment. The ROI is calculated by dividing the total realized profits by the average position size.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The guid returned by AccountGuid(), InputAccount or InputAccountMarket for example. |

**Returns:** `number`

Return the ROI percentage.

---

## GetCurrentProfit

```lua
GetCurrentProfit([direction], [market])
```

Gets unrealized plus realized profits of the current open position. Optional parameters fall back to main bot settings. The accountId parameter can take a positionId as well.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `direction` | `enum` | No | Bot position enum. PositionLong, PositionShort or NoPosition. |
| `market` | `string` | No | The guid returned by AccountGuid(), InputAccount or InputAccountMarket for example. |

**Returns:** `number`

Returns the total profit. On spot/margin the result will be in the quote currency. For leverage its in the underlying currency.

---

## GetCurrentROI

```lua
GetCurrentROI([direction], [market])
```

Gets the return on investment percentage based on the total cost/margin used of the open positions. Optional parameters fall back to main bot settings. The accountId parameter can take a positionId as well.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `direction` | `enum` | No | Bot position enum. PositionLong, PositionShort or NoPosition. |
| `market` | `string` | No | The guid returned by AccountGuid(), InputAccount or InputAccountMarket for example. |

**Returns:** `number`

Returns the ROI percentage.

---

## GetTradingReport

```lua
GetTradingReport([market])
```

Gets the trading report container with many useful values to help properly evaluate a strategy or a trading system.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The guid returned by AccountGuid(), InputAccount or InputAccountMarket for example. |

**Returns:** `dynamic`

The trading report container.

---

## SetBotRoiBaseValue

```lua
SetBotRoiBaseValue(baseValue, [market])
```

Sets a custom base value used in the ROI calculation (realized profits / baseValue * 100).

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `baseValue` | `number` | Yes | The base value for the ROI calculation |
| `market` | `string` | No | The guid returned by AccountGuid(), InputAccount or InputAccountMarket for example. |

**Returns:** `void`

---


# Time Information

## AdjustTimestamp

```lua
AdjustTimestamp([unix], [addSeconds], [addMinutes], [addHours], [addDays], [addMonths], [addYears])
```

Adjusts an Unix timestamp.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `unix` | `number` | No | The unix timestamp, by default the current time.. |
| `addSeconds` | `number` | No | Number of seconds to add. |
| `addMinutes` | `number` | No | Number of minutes to add. |
| `addHours` | `number` | No | Number of hours to add. |
| `addDays` | `number` | No | Number of days to add. |
| `addMonths` | `number` | No | Number of months to add. |
| `addYears` | `number` | No | Number of years to add. |

**Returns:** `number`

Returns the new Unix timestamp.

---

## CloseTime

```lua
CloseTime(unix, interval)
```

Converts a timestamps to the close timestamp of the candle.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `unix` | `number` | Yes | The timestamp to round. Set on 0 for current. |
| `interval` | `number` | Yes | The interval on which the candle is build. |

**Returns:** `number`

Returns the close timestamp of the candle.

---

## CreateTimestamp

```lua
CreateTimestamp([year], [month], [day], [hour], [minute], [second])
```

Creates an Unix timestamp from the specified value.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `year` | `number` | No | The year of the timestamp, by default the current time. |
| `month` | `number` | No | The month of the timestamp, by default the current time. |
| `day` | `number` | No | The date of the timestamp, by default the current time. |
| `hour` | `number` | No | The hour of the timestamp, by default the current time. |
| `minute` | `number` | No | The minute of the timestamp, by default the current time. |
| `second` | `number` | No | The seconds of the timestamp, by default the current time. |

**Returns:** `number`

Returns the Unix timestamp.

---

## CurrentDate

```lua
CurrentDate([unix])
```

Gets the date of the month of the current time or requested time. Based on UTC.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `unix` | `number` | No | An unix timestamp, by default the current time. |

**Returns:** `number`

Returns the date of the month value. A number from 1 to 31.

---

## CurrentDay

```lua
CurrentDay([unix])
```

Gets the day of the week of the current time or requested time. Based on UTC.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `unix` | `number` | No | An unix timestamp, by default the current time. |

**Returns:** `number`

Returns the day of the week value. A number from 1 to 7, starting on Sunday.

---

## CurrentHour

```lua
CurrentHour([unix])
```

Gets the hour the current time or requested time. Based on UTC.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `unix` | `number` | No | An unix timestamp, by default the current time. |

**Returns:** `number`

Returns the hour value. A number from 0 to 23.

---

## CurrentMinute

```lua
CurrentMinute([unix])
```

Gets the minute the current time or requested time. Based on UTC.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `unix` | `number` | No | An unix timestamp, by default the current time. |

**Returns:** `number`

Returns the minute value. A number from 0 to 59.

---

## CurrentMonth

```lua
CurrentMonth([unix])
```

Gets the month of the year of the current time or requested time. Based on UTC.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `unix` | `number` | No | An unix timestamp, by default the current time. |

**Returns:** `number`

Returns the month of the year value. A number from 1 to 12.

---

## CurrentSecond

```lua
CurrentSecond([unix])
```

Gets the second the current time or requested time. Based on UTC.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `unix` | `number` | No | An unix timestamp, by default the current time. |

**Returns:** `number`

Returns the second value. A number from 0 to 59.

---

## CurrentWeek

```lua
CurrentWeek([unix])
```

Gets the week of the year of the current time or requested time. Based on UTC.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `unix` | `number` | No | An unix timestamp, by default the current time. |

**Returns:** `number`

Returns the week of the year value. A number from 1 to 53.

---

## CurrentYear

```lua
CurrentYear([unix])
```

Gets the year the current time or requested time. Based on UTC.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `unix` | `number` | No | An unix timestamp, by default the current time. |

**Returns:** `number`

Returns the year.

---

## FormatDateTime

```lua
FormatDateTime([unix], [dateDelimiter], [dateTimeDelimiter], [timeDelimiter], [includeSeconds], [includeTime], [includeYear])
```

Formats a unix datetime number (to "YYYY-MM-DD at HH:MM" by default).

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `unix` | `number` | No | The unix datetime number to format. |
| `dateDelimiter` | `string` | No | The characters between the year and month, and between the month and day. |
| `dateTimeDelimiter` | `string` | No | The characters between the date and time. |
| `timeDelimiter` | `string` | No | The characters between the hour and minute, and between the minute and second. |
| `includeSeconds` | `boolean` | No | If true, the seconds will be included in the formatted time. |
| `includeTime` | `boolean` | No | If true, the formatted time will be included. |
| `includeYear` | `boolean` | No | If true, the year will be included in the formatted date. |

**Returns:** `string`

The formatted unix datetime number.

---

## GetTimer

```lua
GetTimer([key])
```

Gets the elapsed time for a timer.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `key` | `string` | No | Optional key if using multiple timers. |

**Returns:** `number`

Returns the elapsed time in milliseconds.

---

## MinutesTillCandleClose

```lua
MinutesTillCandleClose(interval)
```

Calculates the number of minutes before a new candle opens.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `interval` | `number` | Yes | Interval in minutes. |

**Returns:** `number`

Returns the number of minutes before a new candle opens.

---

## OpenTime

```lua
OpenTime(unix, interval)
```

Converts a timestamps to the open timestamp of the candle

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `unix` | `number` | Yes | The timestamp to round. Set on 0 for current |
| `interval` | `number` | Yes | The interval on which the candle is build |

**Returns:** `number`

Returns the open timestamp of the candle.

---

## StartTimer

```lua
StartTimer([key])
```

Starts a timer.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `key` | `string` | No | Optional key if using multiple timers. |

**Returns:** `void`

---

## Sleep

```lua
Sleep(milliseconds)
```

Pauses script execution for a specified number of milliseconds. Useful for adding random delays to avoid predictable order timing patterns.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `milliseconds` | `number` | Yes | Number of milliseconds to sleep (e.g., 100 = 100ms, 5000 = 5 seconds). Maximum 10000ms (10 seconds). |

**Returns:** `number`

Returns the actual time slept in milliseconds.

**Note:** Sleep time does not count toward script execution timeout, allowing multiple Sleep() calls without triggering warnings.

**Example - Anti-Detection Order Timing:**

```lua
-- Add random delay to avoid predictable patterns
local rsi = RSI(GetClosePrices(), 14)

if rsi.Value < 30 then
    local delayMs = Random(0, 9000).Value  -- 0-9 seconds
    Log("Delaying order by " .. delayMs .. "ms")
    Sleep(delayMs)
    DoLong()
end
```

---

## StopTimer

```lua
StopTimer([key])
```

Stops a timer.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `key` | `string` | No | Optional key if using multiple timers. |

**Returns:** `number`

Returns the elapsed time in milliseconds.

---

## Time

```lua
Time()
```

Gets the current unix timestamp. Based on UTC.

**Returns:** `number`

Returns the current unix timestamp.

---


# Wallet

## Balance

```lua
Balance([accountId], [coin], [market])
```

Total balance specifications of a specific currency. Available, locked and total balance. Does not work yet in backtests and for simulated accounts.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `accountId` | `string` | No | The guid returned by AccountGuid(), InputAccount() or InputAccountMarket() for example. Can be left empty for default properties. |
| `coin` | `string` | No | Coin of which to retrieve the wallet amount. Default value is the 'UnderlyingAsset' currency. |
| `market` | `string` | No | Optional market parameter for exchanges with separates wallet for each market. |

**Returns:** `dynamic`

Returns a result object with the available, locked and total balance.

---

## BalanceAmount

```lua
BalanceAmount([accountId], [coin], [market])
```

Total balance value of a specific currency. Including order and position margin. Does not work yet in backtests and for simulated accounts.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `accountId` | `string` | No | The guid returned by AccountGuid(), InputAccount() or InputAccountMarket() for example. Can be left empty for default properties. |
| `coin` | `string` | No | Coin of which to retrieve the wallet amount. Default value is the 'UnderlyingAsset' currency. |
| `market` | `string` | No | Optional market parameter for exchanges with separates wallet for each market. |

**Returns:** `number`

Returns the total balance value.

---

## MarginToTradeAmount

```lua
MarginToTradeAmount(price, margin, leverage, [market])
```

Convert margin to a trade amount. In spot markets, the margin is anticipated to be in the quote currency (e.g., USDT in BTC/USDT). For leverage markets, the margin is expected to be in the underlying currency.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `price` | `number` | Yes | The price used in the conversion from margin to trade amount. |
| `margin` | `number` | Yes | The margin used to calculate the trade amount |
| `leverage` | `number` | Yes | Leverage ratio. Use 0 for cross-margin. |
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |

**Returns:** `number`

Returns the calculated trade amount based on the margin.

---

## MaxExitLongAmount

```lua
MaxExitLongAmount([market])
```

Calculates the maximum amount which can be used to place an exit long order based on wallet data. Assets reserved in open orders are excluded by default.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The guid returned by AccountGuid(), InputAccount or InputAccountMarket for example. |

**Returns:** `number`

Returns the maximum exit long amount.

---

## MaxExitShortAmount

```lua
MaxExitShortAmount([market])
```

Calculates the maximum amount which can be used to place an exit short order based on wallet data. Assets reserved in open orders are excluded by default.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The guid returned by AccountGuid(), InputAccount or InputAccountMarket for example. |

**Returns:** `number`

Returns the maximum exit short amount.

---

## MaxLongAmount

```lua
MaxLongAmount([market])
```

Calculates the maximum amount which can be used to place a long order based on wallet data.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The guid returned by AccountGuid(), InputAccount or InputAccountMarket for example. |

**Returns:** `number`

Returns the maximum long amount.

---

## MaxShortAmount

```lua
MaxShortAmount([market])
```

Calculates the maximum amount which can be used to place a short order based on wallet data.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The guid returned by AccountGuid(), InputAccount or InputAccountMarket for example. |

**Returns:** `number`

Returns the maximum long amount.

---

## UserPositionContainer

```lua
UserPositionContainer([accountId], [market], [leverage], [direction])
```

Gets all the information from the open user position.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `accountId` | `string` | No | The guid returned by AccountGuid(), InputAccount or InputAccountMarket for example. |
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |
| `leverage` | `number` | No | Deprecated, Can be ignored. |
| `direction` | `enum` | No | Direction of the position PositionLong or PositionShort. Empty for any. If there is both a long and short position, the long position will be returned. |

**Returns:** `dynamic`

Returns an array with position information.

---

## WalletAmount

```lua
WalletAmount([accountId], [coin], [market])
```

Get the available amount in the wallet.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `accountId` | `string` | No | The guid returned by AccountGuid(), InputAccount() or InputAccountMarket() for example. Can be left empty for default properties. |
| `coin` | `string` | No | Coin of which to retrieve the wallet amount. |
| `market` | `string` | No | Optional market parameter for exchanges with separates wallet for each market. |

**Returns:** `number`

Returns the available amount.

---

## WalletCheck

```lua
WalletCheck(accountId, coin, amount)
```

Checks if the wallet has enough of the funds.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `accountId` | `string` | Yes | The guid returned by AccountGuid(), InputAccount() or InputAccountMarket() for example. Can be left empty for default properties. |
| `coin` | `string` | Yes | Coin of which to retrieve the wallet amount. |
| `amount` | `number` | Yes | The minimum amount needed. |

**Returns:** `boolean`

Returns true if the wallet has enough funds.

---


*Auto-generated from HaasScript source code.*
