# Trading & Positions

139 commands across 11 categories.

## Categories

- [Easy Insurances](#easy-insurances) (14 commands)
- [Easy Safeties](#easy-safeties) (19 commands)
- [Order Handling](#order-handling) (8 commands)
- [Order Information](#order-information) (8 commands)
- [Position Information](#position-information) (14 commands)
- [Position Prices](#position-prices) (11 commands)
- [Signal Helpers](#signal-helpers) (24 commands)
- [Trade Actions (Managed)](#trade-actions-managed) (5 commands)
- [Trade Actions (Unmanaged)](#trade-actions-unmanaged) (8 commands)
- [Trade Bot](#trade-bot) (4 commands)
- [Trade Market Information](#trade-market-information) (24 commands)

---

# Easy Insurances

## AbsolutePriceChange

```lua
AbsolutePriceChange([amount], [targetPrice], [positionId])
```

Compares the average entry price to current price, absolute value-based.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `amount` | `number` | No | Minimum absolute price change before the insurance agrees. |
| `targetPrice` | `number` | No | The target price of the trade. Default is the current buy or sell price. |
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |

**Returns:** `boolean`

Returns true if the minimum price change percentage is reached.

---

## DisableOnLosses

```lua
DisableOnLosses(tradeCount, [acceptedLoss], [market])
```

Blocks new orders if total gain is negative after X trades .

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `tradeCount` | `number` | Yes | The trade count, after which the profits start to matter. |
| `acceptedLoss` | `number` | No | The accepted loss percentage, 0 by default. |
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |

**Returns:** `boolean`

Returns true if completed trades less than [tradeCount] or when gains are positive, otherwise false.

---

## NeverEnterWithALoss

```lua
NeverEnterWithALoss([acceptedLoss], [targetPrice])
```

Blocks enter trade when targetPrice is above the last sell order. Only works on spot and when there is no position open.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `acceptedLoss` | `number` | No | The accepted loss in percentage. Default is 0. This also affects user-defined targetPrice. |
| `targetPrice` | `number` | No | The target price of the trade. Can be used when placing orders beforehand in unmanaged trading. |

**Returns:** `boolean`

Returns true if the trade is allowed.

---

## NeverExitWithLoss

```lua
NeverExitWithLoss([acceptedLoss], [targetPrice], [positionId])
```

Compares the average enter price to the targetPrice or current price.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `acceptedLoss` | `number` | No | The accepted loss in percentage. Default is 0. This also affects user-defined targetPrice. |
| `targetPrice` | `number` | No | The target price of the trade. Can be used when placing orders beforehand in unmanaged trading. |
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |

**Returns:** `boolean`

Returns true if the trade is allowed.

---

## OrderOncePerBar

```lua
OrderOncePerBar([interval], [positionId])
```

Only allows a order to occur once per bar. Signals produced during a ordered bar are blocked.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `interval` | `number` | No | Interval of the bar in minutes. |
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |

**Returns:** `boolean`

Returns true when new bar opens or no orders occurred during a bar.

---

## OvercomeDoubleFeeCosts

```lua
OvercomeDoubleFeeCosts([targetPrice], [positionId])
```

Calculates the target profit and makes sure the fee costs are double-covered.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `targetPrice` | `number` | No | The target price of the trade. Default is the current buy or sell price. |
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |

**Returns:** `boolean`

Returns true if the trade profit is enough to cover double the fees.

---

## OvercomeFeeCosts

```lua
OvercomeFeeCosts([targetPrice], [positionId])
```

Calculates the target profit and makes sure the fee costs are covered.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `targetPrice` | `number` | No | The target price of the trade. Default is the current buy or sell price. |
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |

**Returns:** `boolean`

Returns true if the trade profit is enough to cover the fees.

---

## PercentagePriceChange

```lua
PercentagePriceChange([percentage], [targetPrice], [positionId])
```

Compares the average enter price to current price, percentage-based.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `percentage` | `number` | No | Minimum percentage change before the insurance agrees. |
| `targetPrice` | `number` | No | The target price of the trade. Default is the current buy or sell price. |
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |

**Returns:** `boolean`

Returns true if the minimum price change percentage is reached.

---

## StopLossCooldown

```lua
StopLossCooldown(minutes, [positionId])
```

Blocks any signal until a certain number of minutes have passed since the last exit trade executed by a safety.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `minutes` | `number` | Yes | Number of minutes to wait after the last order. |
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |

**Returns:** `boolean`

Returns true when the last trade is a certain number of minutes ago.

---

## TradeOncePerBar

```lua
TradeOncePerBar([interval], [positionId])
```

Only allows a trade to occur once per bar. Signals produced during a traded bar are blocked.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `interval` | `number` | No | Interval of the bar in minutes. |
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |

**Returns:** `boolean`

Returns true when new bar opens or no trade occurred during a bar.

---

## TradeOnlySideways

```lua
TradeOnlySideways([threshold], [interval], [fullCandles], [market])
```

Checks the current market condition for sideways movement.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `threshold` | `number` | No | Sideways movement threshold. |
| `interval` | `number` | No | The ticks interval. |
| `fullCandles` | `boolean` | No | If true, the last closed candles will be returned. |
| `market` | `string` | No | The market of the tick. Default is the selected main market. |

**Returns:** `boolean`

Returns true if the trade is allowed.

---

## TradeOnlyTrending

```lua
TradeOnlyTrending([threshold], [interval], [fullCandles], [market])
```

Checks the current market condition for a trend.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `threshold` | `number` | No | Trend strength threshold. |
| `interval` | `number` | No | The ticks interval. |
| `fullCandles` | `boolean` | No | If true, the last closed candles will be returned. |
| `market` | `string` | No | The market of the tick. Default is the selected main market. |

**Returns:** `boolean`

Returns true if the trade is allowed.

---

## WaitAfterOrder

```lua
WaitAfterOrder(timeout, [positionId])
```

Blocks any signal until a certain number of minutes have passed since the last placed order. This includes orders executed in an update cycle

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `timeout` | `number` | Yes | Number of minutes to wait after the last order. |
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |

**Returns:** `boolean`

Returns true when the last placed order is a certain number of minutes ago.

---

## WaitAfterTrade

```lua
WaitAfterTrade(timeout, [positionId])
```

Blocks any signal until a certain number of minutes have passed since the last trade.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `timeout` | `number` | Yes | Number of minutes to wait after the last trade. |
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |

**Returns:** `boolean`

Returns true when the last trade is a certain number of minutes ago.

---


# Easy Safeties

## ChandelierStopLoss

```lua
ChandelierStopLoss([depth], [multiplier], [positionId], [direction])
```

Calculates the chandelier exit long/short price and compares it with the current price.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `depth` | `number` | No | Depth length. Default is 20. |
| `multiplier` | `number` | No | ATR multiplier. Default is 3. |
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |
| `direction` | `enum` | No | The direction of the position. PositionLong or PositionShort. By default both. |

**Returns:** `boolean`

Returns true if the stop loss price have been breached.

---

## DeactivateAfterEnterOrder

```lua
DeactivateAfterEnterOrder([positionCount])
```

Deactivates the bot after an enter order has been completed.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `positionCount` | `number` | No | Number of position allowed before the check is preformed. |

**Returns:** `boolean`

Returns true when the bot has been deactivated.

---

## DeactivateAfterExitOrder

```lua
DeactivateAfterExitOrder([positionCount])
```

Deactivates the bot after an exit order has been completed.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `positionCount` | `number` | No | Number of position allowed before the check is preformed. |

**Returns:** `boolean`

Returns true when the bot has been deactivated.

---

## DeactivateAfterXActiveMinutes

```lua
DeactivateAfterXActiveMinutes(minutes)
```

Deactivates the bot when the is active for a certain time.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `minutes` | `number` | Yes | Number of positions allowed before the bot is deactivated. |

**Returns:** `boolean`

Returns true when the bot has been deactivated.

---

## DeactivateAfterXIdleMinutes

```lua
DeactivateAfterXIdleMinutes(minutes)
```

Deactivates the bot if the last completed order is a number of minutes in the past. Timeout starts after the first completed order.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `minutes` | `number` | Yes | Number of positions allowed before the bot is deactivated. |

**Returns:** `boolean`

Returns true when the bot has been deactivated.

---

## DeactivateAfterXOrders

```lua
DeactivateAfterXOrders([count])
```

Deactivates the bot after a number of orders has been completed.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `count` | `number` | No | Number of orders allowed before the bot is deactivated. |

**Returns:** `boolean`

Returns true when the bot has been deactivated.

---

## DeactivateAfterXPositions

```lua
DeactivateAfterXPositions([count])
```

Deactivates the bot after a number of positions has been completed.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `count` | `number` | No | Number of positions allowed before the bot is deactivated. |

**Returns:** `boolean`

Returns true when the bot has been deactivated.

---

## DeactivateOnLoss

```lua
DeactivateOnLoss(maxLoss)
```

Deactivates the bot when there is no open position and the ROI based losses have reached a certain level.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `maxLoss` | `number` | Yes | Maximum allowed loss based on the ROI. |

**Returns:** `boolean`

Returns true when the bot has been deactivated.

---

## DeactivateOnProfit

```lua
DeactivateOnProfit(minProfit)
```

Deactivates the bot when there is no open position and the ROI based profits have reached a certain level.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `minProfit` | `number` | Yes | Minimum profit based on the ROI. |

**Returns:** `boolean`

Returns true when the bot has been deactivated.

---

## DynamicStopLoss

```lua
DynamicStopLoss(percentage, [depth], [positionId], [direction])
```

Calculates the dynamic stop loss price and compares it with the current exit price.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `percentage` | `number` | Yes | Price pump percentage. |
| `depth` | `number` | No | Read depth. Default is 30 ticks. |
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |
| `direction` | `enum` | No | The direction of the position. PositionLong or PositionShort. By default both. |

**Returns:** `boolean`

Returns true is the stop loss price has been breached.

---

## DynamicTakeProfit

```lua
DynamicTakeProfit(percentage, [depth], [positionId], [direction])
```

Calculates the dynamic take profit price and compares it with the current exit price.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `percentage` | `number` | Yes | Price pump percentage. |
| `depth` | `number` | No | Read depth. Default is 30 ticks. |
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |
| `direction` | `enum` | No | The direction of the position. PositionLong or PositionShort. By default both. |

**Returns:** `boolean`

Returns true is the take profit price has been breached.

---

## GrowingTrailingStopLoss

```lua
GrowingTrailingStopLoss(percentage, growPercentage, [positionId], [direction])
```

Keeps track of the highest/lowest recorded price and adjust the stop loss accordingly. The higher the price change the more the trailing distances grows.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `percentage` | `number` | Yes | Trailing stop loss percentage. |
| `growPercentage` | `number` | Yes | The grow percentage. 0.035 for example. |
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |
| `direction` | `enum` | No | The direction of the position. PositionLong or PositionShort. By default both. |

**Returns:** `boolean`

Returns true if the stop loss has been hit.

---

## ShrinkingTrailingStopLoss

```lua
ShrinkingTrailingStopLoss(percentage, shrinkPercentage, [positionId], [direction])
```

Keeps track of the highest/lowest recorded price and adjust the stop loss accordingly. The higher the price change the more the trailing distances shrinks.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `percentage` | `number` | Yes | Trailing stop loss percentage. |
| `shrinkPercentage` | `number` | Yes | This percentage will be added to the stop every time the stop adjust. The percentage will raise exponentially on every adjustment. |
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |
| `direction` | `enum` | No | The direction of the position. PositionLong or PositionShort. By default both. |

**Returns:** `boolean`

Returns true if the stop loss has been hit.

---

## StopLoss

```lua
StopLoss(percentage, [positionId], [direction])
```

Calculates the current percentage change from the average enter price and compares it with the stop loss value.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `percentage` | `number` | Yes | Stop loss percentage. |
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |
| `direction` | `enum` | No | The direction of the position. PositionLong or PositionShort. By default both. |

**Returns:** `boolean`

Returns true if the stop loss has been hit.

---

## StopLossROI

```lua
StopLossROI(percentage, [positionId], [direction])
```

Calculates the defined position's current ROI percentage and gives a signal if the set loss is reached.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `percentage` | `number` | Yes | Stop loss ROI percentage. |
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |
| `direction` | `enum` | No | The direction of the position. PositionLong or PositionShort. By default both. |

**Returns:** `boolean`

Returns true if the stop loss has been hit.

---

## TakeProfit

```lua
TakeProfit(percentage, [positionId], [direction])
```

Calculates the current percentage change from the average enter price and compares it with the take profit value.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `percentage` | `number` | Yes | Take profit percentage. |
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |
| `direction` | `enum` | No | The direction of the position. PositionLong or PositionShort. By default both. |

**Returns:** `boolean`

Returns true if the take profit has been reached.

---

## TakeProfitROI

```lua
TakeProfitROI(percentage, [positionId], [direction])
```

Calculates the defined position's current ROI percentage and gives a signal if the set profit is reached.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `percentage` | `number` | Yes | Take profit ROI percentage. |
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |
| `direction` | `enum` | No | The direction of the position. PositionLong or PositionShort. By default both. |

**Returns:** `boolean`

Returns true if the take profit has been reached.

---

## TrailingArmStopLoss

```lua
TrailingArmStopLoss(percentage, arm, [positionId], [direction])
```

Keeps track of the highest/lowest recorded price and adjust the stop loss accordingly when the arm% has been reached.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `percentage` | `number` | Yes | Trailing stop loss percentage. |
| `arm` | `number` | Yes | The Arm percentage. |
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |
| `direction` | `enum` | No | The direction of the position. PositionLong or PositionShort. By default both. |

**Returns:** `boolean`

Returns true if the stop loss has been hit.

---

## TrailingStopLoss

```lua
TrailingStopLoss(percentage, [positionId], [direction])
```

Keeps track of the highest/lowest recorded price and adjust the stop loss accordingly.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `percentage` | `number` | Yes | Trailing stop loss percentage. |
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |
| `direction` | `enum` | No | The direction of the position. PositionLong or PositionShort. By default both. |

**Returns:** `boolean`

Returns true if the stop loss has been hit.

---


# Order Handling

## CancelAllOrders

```lua
CancelAllOrders([positionId])
```

Cancels all open orders.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |

**Returns:** `void`

---

## CancelOrder

```lua
CancelOrder(orderId)
```

Sends out a cancel order for the given order identifier.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `orderId` | `string` | Yes | Unique order identifier provided by the PlaceOrder commands. |

**Returns:** `void`

---

## IsAnyOrderFinished

```lua
IsAnyOrderFinished([positionId])
```

Checks if there are any open orders completed/cancelled.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |

**Returns:** `boolean`

Returns true if there are any open orders completed/cancelled.

---

## IsAnyOrderOpen

```lua
IsAnyOrderOpen([positionId])
```

Checks if there are any open orders.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |

**Returns:** `boolean`

Returns true if there are any open orders.

---

## IsBuyOrder

```lua
IsBuyOrder(orderId)
```

Checks if the given order is a buy order.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `orderId` | `string` | Yes | Unique order identifier provided by the PlaceOrder commands. |

**Returns:** `boolean`

Returns true if the given order is on buy side.

---

## IsOrderFilled

```lua
IsOrderFilled(orderId)
```

Checks if the given order is filled.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `orderId` | `string` | Yes | Unique order identifier provided by the PlaceOrder commands. |

**Returns:** `boolean`

Returns true if the given order is filled.

---

## IsOrderOpen

```lua
IsOrderOpen(orderId)
```

Checks if the given order is still open.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `orderId` | `string` | Yes | Unique order identifier provided by the PlaceOrder commands. |

**Returns:** `boolean`

Returns true if the given order is still open.

---

## IsSellOrder

```lua
IsSellOrder(orderId)
```

Checks if the given order is a sell order.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `orderId` | `string` | Yes | Unique order identifier provided by the PlaceOrder commands. |

**Returns:** `boolean`

Returns true if the given order is on sell side.

---


# Order Information

## GetAllFilledOrders

```lua
GetAllFilledOrders([positionId])
```

A collection of (partially) filled orders executed by the bot. Maximum size is 50 orders.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `positionId` | `string` | No | Optional unique identifier. When set only orders related to that position are returned. |

**Returns:** `dynamic`

Returns a list of OrderContainer() objects. Sorted descending on close timestamp.

---

## GetAllOpenOrders

```lua
GetAllOpenOrders([positionId])
```

A collection of open orders executed by the bot.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `positionId` | `string` | No | Optional unique identifier. When set only orders related to that position are returned. |

**Returns:** `dynamic`

Returns a list of OrderContainer() objects.

---

## GetFailedOrderMessage

```lua
GetFailedOrderMessage(orderId)
```

The reason for the order failure.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `orderId` | `string` | Yes | Unique order identifier provided by the PlaceOrder commands. |

**Returns:** `string`

Returns a string with the reason for the order failure.

---

## GetOrderCancelledAmount

```lua
GetOrderCancelledAmount(orderId)
```

Calculates the cancelled amount if the order is completed/cancelled.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `orderId` | `string` | Yes | Unique order identifier provided by the PlaceOrder commands. |

**Returns:** `number`

Returns the cancelled amount if the order is completed/cancelled.

---

## GetOrderFilledAmount

```lua
GetOrderFilledAmount(orderId, [afterFees])
```

Calculates the amount filled for a specific order.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `orderId` | `string` | Yes | Unique order identifier provided by the PlaceOrder commands. |
| `afterFees` | `boolean` | No | If true, the fee cost are subtracted from the total amount if they are paid in the base currency. |

**Returns:** `number`

Returns the amount filled for a specific order.

---

## GetOrderOpenTime

```lua
GetOrderOpenTime(orderId, [inSeconds])
```

Calculates how long an order is or has been open in minutes.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `orderId` | `string` | Yes | Unique order identifier provided by the PlaceOrder commands. |
| `inSeconds` | `boolean` | No | If set on true, the timespan will be in seconds. |

**Returns:** `number`

Returns the time the order is or has been open for in minutes.

---

## GetOrderProfit

```lua
GetOrderProfit(orderId)
```

Calculates the realized profits of the order.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `orderId` | `string` | Yes | Unique order identifier provided by the PlaceOrder commands. |

**Returns:** `number`

Returns gross profits of the order.

---

## OrderContainer

```lua
OrderContainer(orderId)
```

Gets all the information from an specific order.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `orderId` | `string` | Yes | Unique order identifier provided by the PlaceOrder commands. |

**Returns:** `dynamic`

Returns an array with order information.

---


# Position Information

## AdjustVPosition

```lua
AdjustVPosition(price, amount, [positionId])
```

Adjusts a virtual bot position by processing a simulated order using the defined specifications. Works similarly to PlaceExitLongOrder/PlaceExitShortOrder, but will NOT create a real order; only simulated!

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `price` | `number` | Yes | The close price of the position. |
| `amount` | `number` | Yes | The amount to adjust: negative value reduces, positive value adds. |
| `positionId` | `string` | No | Unique position identifier. |

**Returns:** `void`

---

## CloseVPosition

```lua
CloseVPosition(price, [positionId])
```

Closes a virtual bot position entirely by processing a simulated order using the defined specifications. Works similarly to DoExitPosition and PlaceExitPositionOrder, but will NOT create a real order; only simulated! Use this to "clean up" bot positions if required.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `price` | `number` | Yes | The enter price of the position. |
| `positionId` | `string` | No | Unique position identifier. |

**Returns:** `void`

---

## CreatePosition

```lua
CreatePosition(direction, price, amount, [market], [leverage], [positionId])
```

Creates a bot position by processing a simulated order using the defined specifications. By doing this, profit/loss and position details can be used by the script. Use the returned position ID with PositionContainer() to extract position data.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `direction` | `enum` | Yes | The direction of the position. |
| `price` | `number` | Yes | The enter price of the position. |
| `amount` | `number` | Yes | The size of the position. |
| `market` | `string` | No | The market of the position. |
| `leverage` | `number` | No | The leverage of the position. -1 for default. |
| `positionId` | `string` | No | Unique position identifier. |

**Returns:** `string`

Returns the position ID.

---

## GetAllOpenPositions

```lua
GetAllOpenPositions()
```

Gets all open positions and returns them as PositionContainers.

**Returns:** `dynamic`

Returns an array of PositionContainers.

---

## GetPositionAmount

```lua
GetPositionAmount([positionId])
```

Gets the total amount open in a specific position.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |

**Returns:** `number`

Returns the total amount open in a specific position.

---

## GetPositionDirection

```lua
GetPositionDirection([positionId])
```

Gets the direction of a specific position.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |

**Returns:** `enum`

Returns PositionLong, PositionShort or NoPosition.

---

## GetPositionEnterPrice

```lua
GetPositionEnterPrice([positionId], [includeClosed])
```

Gets the average enter price of an open position.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |
| `includeClosed` | `boolean` | No | True for total position amount (default), false for only open amount. |

**Returns:** `number`

Returns the average enter price.

---

## GetPositionMarket

```lua
GetPositionMarket([positionId])
```

Gets the market of an open position.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |

**Returns:** `string`

Returns the account, market & leverage as a single string.

---

## GetPositionProfit

```lua
GetPositionProfit([positionId], [targetPrice])
```

Gets the total realized plus unrealized profit of a specific position.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |
| `targetPrice` | `number` | No | Optional target price. Default is the best bid/ask. |

**Returns:** `number`

Returns the total realized plus unrealized profit of a specific position. On spot/margin the result will be in the quote currency. For leverage its in the underlying currency.

---

## GetPositionROI

```lua
GetPositionROI([positionId], [targetPrice])
```

Calculates the ROI based on used margin and unrealized + realized profits.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |
| `targetPrice` | `number` | No | Optional target price. Default is the best bid/ask. |

**Returns:** `number`

Returns the ROI as a percentage.

---

## IsPositionClosed

```lua
IsPositionClosed([positionId])
```

Checks whether or not a position is closed.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |

**Returns:** `boolean`

Returns true if position is closed, otherwise false.

---

## LongAmount

```lua
LongAmount([market])
```

Gets the total amount open in a long position.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |

**Returns:** `number`

Returns the total amount open in a long position.

---

## PositionContainer

```lua
PositionContainer([positionId], [includeClosed])
```

Gets all the information from the open bot position.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |
| `includeClosed` | `boolean` | No | True for total position amount (default), false for only open amount. Affects enterPrice value in returned object. |

**Returns:** `dynamic`

Returns an array with position information.

---

## ShortAmount

```lua
ShortAmount([market])
```

Gets the total amount open in a short position.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |

**Returns:** `number`

Returns the total amount open in a short position.

---


# Position Prices

## AverageEnterPrice

```lua
AverageEnterPrice([positionId], [includeClosed])
```

Gets the average enter price.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |
| `includeClosed` | `boolean` | No | True for total position amount (default), false for only open amount. |

**Returns:** `number`

Returns the average enter price.

---

## AverageExitPrice

```lua
AverageExitPrice([positionId])
```

Gets the average exit price.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |

**Returns:** `number`

Returns the average exit price.

---

## LastExitLongPrice

```lua
LastExitLongPrice()
```

Gets the last used exit long price.

**Returns:** `number`

Returns the last used exit long price.

---

## LastExitPositionPrice

```lua
LastExitPositionPrice()
```

Gets the last used exit short or long price.

**Returns:** `number`

Returns the last used exit short or long price.

---

## LastExitShortPrice

```lua
LastExitShortPrice()
```

Gets the last used exit short price.

**Returns:** `number`

Returns the last used exit short price.

---

## LastLongPrice

```lua
LastLongPrice()
```

Gets the last enter long price.

**Returns:** `number`

Returns the last enter long price.

---

## LastLongProfit

```lua
LastLongProfit([positionId])
```

Gets the profit of the last long exit or sell trade.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |

**Returns:** `number`

Returns the profit.

---

## LastLongROI

```lua
LastLongROI([positionId])
```

Gets the last long position ROI.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |

**Returns:** `number`

Returns the last long position ROI.

---

## LastShortPrice

```lua
LastShortPrice()
```

Gets the last enter short price.

**Returns:** `number`

Returns the last enter short price.

---

## LastShortProfit

```lua
LastShortProfit([positionId])
```

Gets the profit of the last short exit or buy trade.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |

**Returns:** `number`

Returns the profit.

---

## LastShortROI

```lua
LastShortROI([positionId])
```

Gets the last short position ROI.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |

**Returns:** `number`

Returns the last short position ROI.

---


# Signal Helpers

## BoolToSignal

```lua
BoolToSignal([isLong], [isShort], [isExit], [isNone])
```

Converts 4 boolean values to a signal.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `isLong` | `boolean` | No | When true, SignalLong will be returned. |
| `isShort` | `boolean` | No | When true, SignalShort will be returned. |
| `isExit` | `boolean` | No | When true, SignalExitPosition will be returned. |
| `isNone` | `boolean` | No | When true, SignalNone will be returned. |

**Returns:** `enum`

The signal as a result of the boolean values.

---

## ConvertSignal

```lua
ConvertSignal(signal, signalToCovert, convertedSignal)
```

**Aliases:** `MapSignal`

Converts the signal if its matches the defined one.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `signal` | `enum` | Yes | The original signal. |
| `signalToCovert` | `enum` | Yes | The signal we want to convert. |
| `convertedSignal` | `enum` | Yes | The signal we want to convert too. |

**Returns:** `enum`

Returns the original or converted signal depending on the input.

---

## DelaySignal

```lua
DelaySignal(signal, minutes)
```

Saves and delays a signal by a number of minutes.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `signal` | `enum` | Yes | The original signal. |
| `minutes` | `number` | Yes | The number of minutes of the delay. |

**Returns:** `enum`

Returns the saved signal from a number of minutes back or SignalNone when it doesn't have a delayed signal yet.

---

## GetAboveBelowSignal

```lua
GetAboveBelowSignal(fast, slow)
```

Returns a continuous signal when values are above/below of each other.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `fast` | `number` | Yes | The 'faster' value. |
| `slow` | `number` | Yes | The 'slower' value. |

**Returns:** `enum`

Returns SignalLong when fast is above slow, SignalShort when fast is below slow and SignalNone will occur if values are exactly the same.

---

## GetBuySellLevelSignal

```lua
GetBuySellLevelSignal(value, buyLevel, sellLevel)
```

Determines the signal from a result with a buy and sell level.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `value` | `number` | Yes | The value to compare. |
| `buyLevel` | `number` | Yes | Below this value, a DoLong() is executed. |
| `sellLevel` | `number` | Yes | Above this value, a DoShort() is executed. |

**Returns:** `enum`

Returns SignalLong when the result is below the buy level, SignalShort when the result is above the sell level and SignalNone when the result between the buy and sell level.

---

## GetConsensusSignal

```lua
GetConsensusSignal(signals[])
```

Counts the number of signals. When the majority agrees it will return the signal.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `signals[]` | `EnumParams` | Yes | Indicator signals collection. Takes a variable number of arguments. |

**Returns:** `enum`

Returns SignalLong, SignalShort, SignalExitPosition or SignalNone when there is a consensus or SignalNone when there is not.

---

## GetCrossOverUnderSignal

```lua
GetCrossOverUnderSignal(fast, slow)
```

Determines the signal from a result with a crossover or -under level.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `fast` | `number|table` | Yes | The 'faster' value. |
| `slow` | `number|table` | Yes | The 'slower' value. |

**Returns:** `enum`

Returns SignalLong when the a crossover occurs, SignalShort when a crossunder occurs and SignalNone when non one those happened.

---

## GetRemoteSignal

```lua
GetRemoteSignal(id)
```

Gets the remote signal which is published by SaveRemoteSignal()

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `id` | `string` | Yes | The unique identifier of the signal. |

**Returns:** `enum`

Returns a trade signal enumeration. For example SignalLong(), SignalShort(), SignalExitPosition(), SignalNone() or SignalError().

---

## GetSuperSignal

```lua
GetSuperSignal(indicatorValue, signalValue, signalType, [centerPosition], [buyLevel], [sellLevel])
```

Returns a continuous trade signal with defined signal type and values.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `indicatorValue` | `HaasNumberCollection` | Yes | The 'faster' or indicator value. |
| `signalValue` | `HaasNumberCollection` | Yes | The 'slower' or signal value. |
| `signalType` | `enum` | Yes | The signal type which defines how the inputs are interpreted. |
| `centerPosition` | `number` | No | The center position for 'Zero Cross' signal types. 0 for indicators that oscillate below and above zero, 50 for indicators like RSI, MFI and STOCH. |
| `buyLevel` | `number` | No | The buy level for 'Threshold' signal types. Used for indicators like RSI, MFI and STOCH. |
| `sellLevel` | `number` | No | The sell level for 'Threshold' signal types. Used for indicators like RSI, MFI and STOCH. |

**Returns:** `enum`

Returns a signal based on defined 'signalType' (see InputSignalTypes()).

---

## GetThresholdSignal

```lua
GetThresholdSignal(value, threshold, [swing])
```

Determines the signal based on a result and a threshold.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `value` | `number` | Yes | The value to compare. |
| `threshold` | `number` | Yes | Below this value, a SignalLong is returned. If its above a SignalShort. |
| `swing` | `number` | No | Minimum swing below or above the threshold before a signal is returned. |

**Returns:** `enum`

Returns SignalLong when the result is below the threshold and SignalShort when the result is above the threshold.

---

## GetUnanimousSignal

```lua
GetUnanimousSignal(signals[])
```

Validates if all signals are the same.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `signals[]` | `EnumParams` | Yes | Indicator signals collection. Takes a variable number of arguments. |

**Returns:** `enum`

Returns SignalLong, SignalShort or SignalNone when there is unanimous decision and SignalNone when there is not.

---

## GetWebHookSignal

```lua
GetWebHookSignal(id)
```

Gets the latest WebHook signal of a specific key.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `id` | `string` | Yes | The unique identifier of the signal. |

**Returns:** `dynamic`

Returns the latest signal object stored by an incoming WebHook.

---

## GetWeightedConsensusSignal

```lua
GetWeightedConsensusSignal(longWeight, shortWeight, exitWeight, signals[])
```

Calculates and determines the weighted consensus signal.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `longWeight` | `number` | Yes | The required sum of the long signals weight before the signal is valid. |
| `shortWeight` | `number` | Yes | The required sum of the short signals weight before the signal is valid. |
| `exitWeight` | `number` | Yes | The required sum of the exit signals weight before the signal is valid. |
| `signals[]` | `EnumParams` | Yes | Indicator signals collection. Takes a variable number of arguments. |

**Returns:** `enum`

Returns SignalLong, SignalShort, SignalExitPosition or SignalNone when there is a consensus or SignalNone when there is not.

---

## HindSightSignal

```lua
HindSightSignal(period)
```

Uses hindsight to tell if its the best moment to long or short

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `period` | `number` | Yes | Hindsight view period length. |

**Returns:** `enum`

Returns a trading signal

---

## IgnoreSignalIf

```lua
IgnoreSignalIf(signal, signalToIgnore)
```

Marks the signal to be ignored when it matches a certain signal.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `signal` | `enum` | Yes | The original signal. |
| `signalToIgnore` | `enum` | Yes | The signal to ignore. SignalLong, SignalShort or SignalExitPosition. |

**Returns:** `enum`

Returns the original signal.

---

## PositionToBool

```lua
PositionToBool(position)
```

Converts a position value to 3 boolean values.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `position` | `enum` | Yes | Bot position enum. PositionLong, PositionShort or NoPosition. |

**Returns:** `dynamic`

Three boolean values based on the position input.

---

## ReverseSignal

```lua
ReverseSignal(signal)
```

Reverses a signal from long to short or short to long. SignalNone will be ignored and stay the same.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `signal` | `enum` | Yes | The original signal. |

**Returns:** `enum`

Returns the reversed signal.

---

## SaveRemoteSignal

```lua
SaveRemoteSignal(id, secret, signal)
```

This command can save and distribute a trading signal over the HaasCloud. This can be used in combination with trade signal enumeration. For example SignalLong(), SignalShort(), SignalExitPosition() or SignalNone().

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `id` | `string` | Yes | The unique identifier of the signal. |
| `secret` | `string` | Yes | The secret key on which to save the signal. |
| `signal` | `enum` | Yes | The signal enum to save. |

**Returns:** `void`

---

## SignalMapper

```lua
SignalMapper(signal, [reverse], [mapLongTo], [mapShortTo], [mapExitTo], [mapNoneTo])
```

Maps a signal value to their defined specifications.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `signal` | `enum` | Yes | The original signal. |
| `reverse` | `boolean` | No | If true, the signals are revered. |
| `mapLongTo` | `enum` | No | The mapped long signal. |
| `mapShortTo` | `enum` | No | The mapped short signal. |
| `mapExitTo` | `enum` | No | The mapped exit signal. |
| `mapNoneTo` | `enum` | No | The mapped none signal. |

**Returns:** `enum`

The mapped signal.

---

## SignalProperties

```lua
SignalProperties(signal, [useLong], [useShort], [useExit], [weight], [delay])
```

Sets properties such as weight or ignore in the signal input.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `signal` | `enum` | Yes | The original signal. |
| `useLong` | `boolean` | No | When true, this signal is used for long decisions. |
| `useShort` | `boolean` | No | When true, this signal is used for short decisions. |
| `useExit` | `boolean` | No | When true, this signal is used for exit decisions. |
| `weight` | `number` | No | The weight of the signal. Minimum weight of 1 required. |
| `delay` | `number` | No | Number of minutes the signal will be delayed. |

**Returns:** `enum`

Returns the signal with the new specifications.

---

## SignalToBool

```lua
SignalToBool(signal)
```

Converts a signal value to 4 boolean values.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `signal` | `enum` | Yes | The original signal. |

**Returns:** `dynamic`

Four boolean values based on the signal input.

---

## SignalToLog

```lua
SignalToLog(signals)
```

Logs the given signals.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `signals` | `EnumParams` | Yes | The signals to log. |

**Returns:** `void`

---

## SignalWeight

```lua
SignalWeight(signal, weight)
```

Add a weight to the signal. Can be used in combination with GetWeightedConsensusSignal().

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `signal` | `enum` | Yes | The original signal. |
| `weight` | `number` | Yes | The weight of the signal. Minimum weight of 1 required. |

**Returns:** `enum`

Returns the original signal with a weight mark.

---

## UseSignalIf

```lua
UseSignalIf(signal, signalToUse)
```

Marks the signal to be used when its matches a certain signal.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `signal` | `enum` | Yes | The original signal. |
| `signalToUse` | `enum` | Yes | The signal to ignore. SignalLong, SignalShort or SignalExitPosition. |

**Returns:** `enum`

Returns the original signal.

---


# Trade Actions (Managed)

## DoExitPosition

```lua
DoExitPosition([note], [count])
```

Sends out a sell signal on spot or an exit positions signal on margin & leverage.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `note` | `string` | No | A note for the order. Visible in the open orders and history. |
| `count` | `number` | No |  |

**Returns:** `void`

---

## DoFlipPosition

```lua
DoFlipPosition([note])
```

Executes a DoLong() when the current position is short and an DoShort() the position is long.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `note` | `string` | No | A note for the order. Visible in the open orders and history. |

**Returns:** `void`

---

## DoLong

```lua
DoLong([note], [count])
```

**Aliases:** `DoBuy`

Generates a buy signal on spot or a long signal on margin & leverage.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `note` | `string` | No | A note for the order. Visible in the open orders and history. |
| `count` | `number` | No |  |

**Returns:** `void`

---

## DoShort

```lua
DoShort([note], [count])
```

**Aliases:** `DoSell`

Generates a sell signal on spot or a short signal on margin & leverage.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `note` | `string` | No | A note for the order. Visible in the open orders and history. |
| `count` | `number` | No |  |

**Returns:** `void`

---

## DoSignal

```lua
DoSignal(signal, [note])
```

Processes a signal enum. Triggers a DoLong() when the signal equals SignalLong, a DoShort() when the signal equals SignalShort, a DoExitPosition() when the signal equals SignalExitPosition and does nothing when the signal is SignalNone.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `signal` | `enum` | Yes | The signal to process. |
| `note` | `string` | No | A note for the order. Visible in the open orders and history. |

**Returns:** `void`

---


# Trade Actions (Unmanaged)

## PlaceBuyOrder

```lua
PlaceBuyOrder(price, amount, [market], [type], [note], [positionId], [timeout], [triggerPrice], [reduceOnly], [hiddenOrder])
```

Places a (limit) buy order for the given amount and price. Returns the order identifier (text).

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `price` | `number` | Yes | The price on which to execute the order. If the order is a market order, this field will be ignored. |
| `amount` | `number` | Yes | The amount to execute. |
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |
| `type` | `enum` | No | The type of order. Default is limit. |
| `note` | `string` | No | A note for the order. Visible in the open orders and history. |
| `positionId` | `string` | No | Optional unique internal identifier that can be used to maintain multiple separate positions. |
| `timeout` | `number` | No | The order timeout in seconds. By default the timeout is 600 seconds / 10 minutes. Using a negative value will disable the timeout. |
| `triggerPrice` | `number` | No | Trigger price, used for conditional orders. This parameter must only be set when using native order types that are supported by the exchange. |
| `reduceOnly` | `boolean` | No | Ensures that the executed order does not flip the opened position. |
| `hiddenOrder` | `boolean` | No | The hidden order option ensures an order does not appear in the order book. |

**Returns:** `string`

Unique order identifier.

---

## PlaceCancelledOrder

```lua
PlaceCancelledOrder(orderId, [price])
```

Executes the remaining amount of a canceled order.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `orderId` | `string` | Yes | Unique order identifier. Returned by the PlaceOrder commands. |
| `price` | `number` | No | The price on which to execute. Be defaults the best ask or bid price is used. |

**Returns:** `string`

Returns an new order identifier. Empty if no order is executed.

---

## PlaceExitLongOrder

```lua
PlaceExitLongOrder(price, amount, [market], [type], [note], [positionId], [timeout], [triggerPrice], [reduceOnly], [hiddenOrder])
```

Places a (limit) exit long order for the given amount and price. Returns the order identifier (text).

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `price` | `number` | Yes | The price on which to execute the order. If the order is a market order, this field will be ignored. |
| `amount` | `number` | Yes | The amount to execute. |
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |
| `type` | `enum` | No | The type of order. Default is limit. |
| `note` | `string` | No | A note for the order. Visible in the open orders and history. |
| `positionId` | `string` | No | Optional unique internal identifier that can be used to maintain multiple separate positions. |
| `timeout` | `number` | No | The order timeout in seconds. By default the timeout is 600 seconds / 10 minutes. Using a negative value will disable the timeout. |
| `triggerPrice` | `number` | No | Trigger price, used for conditional orders. This parameter must only be set when using native order types that are supported by the exchange. |
| `reduceOnly` | `boolean` | No | Ensures that the executed order does not flip the opened position. |
| `hiddenOrder` | `boolean` | No | The hidden order option ensures an order does not appear in the order book. |

**Returns:** `string`

Unique order identifier.

---

## PlaceExitPositionOrder

```lua
PlaceExitPositionOrder([positionId], [price], [type], [note], [timeout])
```

Places exit orders for a specific bot position.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `positionId` | `string` | No | Optional unique identifier. Required when the bot is trading multiple position at once. |
| `price` | `number` | No | The price on which to execute. Be defaults the best ask or bid price is used. |
| `type` | `enum` | No | Order type used for the order. |
| `note` | `string` | No | A note for the order. Visible in the open orders and history. |
| `timeout` | `number` | No | The order timeout in seconds. By default the timeout is 600 seconds / 10 minutes except for NoTimeOutOrderType. |

**Returns:** `string`

Returns an order identifier.

---

## PlaceExitShortOrder

```lua
PlaceExitShortOrder(price, amount, [market], [type], [note], [positionId], [timeout], [triggerPrice], [reduceOnly], [hiddenOrder])
```

Places a (limit) exit short order for the given amount and price. Returns the order identifier (text).

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `price` | `number` | Yes | The price on which to execute the order. If the order is a market order, this field will be ignored. |
| `amount` | `number` | Yes | The amount to execute. |
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |
| `type` | `enum` | No | The type of order. Default is limit. |
| `note` | `string` | No | A note for the order. Visible in the open orders and history. |
| `positionId` | `string` | No | Optional unique internal identifier that can be used to maintain multiple separate positions. |
| `timeout` | `number` | No | The order timeout in seconds. By default the timeout is 600 seconds / 10 minutes. Using a negative value will disable the timeout. |
| `triggerPrice` | `number` | No | Trigger price, used for conditional orders. This parameter must only be set when using native order types that are supported by the exchange. |
| `reduceOnly` | `boolean` | No | Ensures that the executed order does not flip the opened position. |
| `hiddenOrder` | `boolean` | No | The hidden order option ensures an order does not appear in the order book. |

**Returns:** `string`

Unique order identifier.

---

## PlaceGoLongOrder

```lua
PlaceGoLongOrder(price, amount, [market], [type], [note], [positionId], [timeout], [triggerPrice], [reduceOnly], [hiddenOrder])
```

Places a (limit) go long order for the given amount and price. Returns the order identifier (text).

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `price` | `number` | Yes | The price on which to execute the order. If the order is a market order, this field will be ignored. |
| `amount` | `number` | Yes | The amount to execute. |
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |
| `type` | `enum` | No | The type of order. Default is limit. |
| `note` | `string` | No | A note for the order. Visible in the open orders and history. |
| `positionId` | `string` | No | Optional unique internal identifier that can be used to maintain multiple separate positions. |
| `timeout` | `number` | No | The order timeout in seconds. By default the timeout is 600 seconds / 10 minutes. Using a negative value will disable the timeout. |
| `triggerPrice` | `number` | No | Trigger price, used for conditional orders. This parameter must only be set when using native order types that are supported by the exchange. |
| `reduceOnly` | `boolean` | No | Ensures that the executed order does not flip the opened position. |
| `hiddenOrder` | `boolean` | No | The hidden order option ensures an order does not appear in the order book. |

**Returns:** `string`

Unique order identifier.

---

## PlaceGoShortOrder

```lua
PlaceGoShortOrder(price, amount, [market], [type], [note], [positionId], [timeout], [triggerPrice], [reduceOnly], [hiddenOrder])
```

Places a (limit) go short order for the given amount and price. Returns the order identifier (text).

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `price` | `number` | Yes | The price on which to execute the order. If the order is a market order, this field will be ignored. |
| `amount` | `number` | Yes | The amount to execute. |
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |
| `type` | `enum` | No | The type of order. Default is limit. |
| `note` | `string` | No | A note for the order. Visible in the open orders and history. |
| `positionId` | `string` | No | Optional unique internal identifier that can be used to maintain multiple separate positions. |
| `timeout` | `number` | No | The order timeout in seconds. By default the timeout is 600 seconds / 10 minutes. Using a negative value will disable the timeout. |
| `triggerPrice` | `number` | No | Trigger price, used for conditional orders. This parameter must only be set when using native order types that are supported by the exchange. |
| `reduceOnly` | `boolean` | No | Ensures that the executed order does not flip the opened position. |
| `hiddenOrder` | `boolean` | No | The hidden order option ensures an order does not appear in the order book. |

**Returns:** `string`

Unique order identifier.

---

## PlaceSellOrder

```lua
PlaceSellOrder(price, amount, [market], [type], [note], [positionId], [timeout], [triggerPrice], [reduceOnly], [hiddenOrder])
```

Places a (limit) sell order for the given amount and price. Returns the order identifier (text).

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `price` | `number` | Yes | The price on which to execute the order. If the order is a market order, this field will be ignored. |
| `amount` | `number` | Yes | The amount to execute. |
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |
| `type` | `enum` | No | The type of order. Default is limit. |
| `note` | `string` | No | A note for the order. Visible in the open orders and history. |
| `positionId` | `string` | No | Optional unique internal identifier that can be used to maintain multiple separate positions. |
| `timeout` | `number` | No | The order timeout in seconds. By default the timeout is 600 seconds / 10 minutes. Using a negative value will disable the timeout. |
| `triggerPrice` | `number` | No | Trigger price, used for conditional orders. This parameter must only be set when using native order types that are supported by the exchange. |
| `reduceOnly` | `boolean` | No | Ensures that the executed order does not flip the opened position. |
| `hiddenOrder` | `boolean` | No | The hidden order option ensures an order does not appear in the order book. |

**Returns:** `string`

Unique order identifier.

---


# Trade Bot

## IndicatorContainer

```lua
IndicatorContainer([signals[]])
```

Merges multiple signals into a single list and determines the unanimous and consensus signal.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `signals[]` | `EnumParams` | No | Indicator signals. Takes a variable number of arguments. |

**Returns:** `dynamic`

Returns a list with on index 1 the array with signals, on index 2 the unanimous and on index 3 the consensus signal.

---

## InsuranceContainer

```lua
InsuranceContainer([signals[]])
```

Validates if all the signals agree.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `signals[]` | `...boolean` | No | Safety signals collection. Takes a variable number of arguments. |

**Returns:** `boolean`

Returns true if all the signals in the collection are true.

---

## SafetyContainer

```lua
SafetyContainer([signals[]])
```

Checks a collection for active safety signals.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `signals[]` | `...boolean` | No | Safety signals collection. Takes a variable number of arguments. |

**Returns:** `boolean`

Returns true if any of the signals is active.

---

## TradeBotContainer

```lua
TradeBotContainer([safetySignal], [indicatorSignal], [insuranceSignal])
```

Checks the safety, indicator & insurance signal. If a safety is active, DoExitPosition() will be executed. When there are no safeties active, the indicator signal will be executed if the insurances agree.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `safetySignal` | `boolean` | No | Safety container result. |
| `indicatorSignal` | `enum` | No | Indicator container result. |
| `insuranceSignal` | `boolean` | No | Insurance container result. |

**Returns:** `void`

---


# Trade Market Information

## AmountDecimals

```lua
AmountDecimals([market], [amount])
```

Returns a number of how many decimals is allowed for order amounts.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |
| `amount` | `number` | No |  |

**Returns:** `number`

Returns the allowed decimal count.

---

## AmountLabel

```lua
AmountLabel([market])
```

**Aliases:** `AmountCurrency`

Gets the amount label. For reflection, in case the market is Bitstamp BTC/USDit will return 'BTC'. When we are on BitMEX XBT/USD (XBTUSD), it will return 'contract(s)'.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |

**Returns:** `string`

Returns the amount label as string.

---

## AmountStep

```lua
AmountStep([market])
```

Returns the minimum allowed trade amount step for market.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |

**Returns:** `number`

Returns the trade amount step size.

---

## BaseCurrency

```lua
BaseCurrency([market])
```

Returns the base currency of the market. For reflection, in case we are on Bitstamp BTC/USD it will return 'BTC'.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |

**Returns:** `string`

Returns the base currency as string.

---

## ContractName

```lua
ContractName([market])
```

Gets the contract name of the market.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |

**Returns:** `string`

Returns the contract name of the market.

---

## ContractValue

```lua
ContractValue([market])
```

Gets the value of a single contract, defined in their denomination.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |

**Returns:** `number`

Returns the value of a single contract, defined in their denomination.

---

## GetAccountMarkets

```lua
GetAccountMarkets([accountId])
```

An array of markets affiliated with a particular account.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `accountId` | `string` | No | The guid returned by AccountGuid(), InputAccount or InputAccountMarket for example. |

**Returns:** `dynamic`

Returns an array with all the markets as a string.

---

## GetExchangeMarkets

```lua
GetExchangeMarkets(exchangeCode)
```

An array of markets affiliated with a particular exchange.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `exchangeCode` | `string` | Yes | The exchange code affiliated with the exchange. |

**Returns:** `dynamic`

Returns an array with all the markets as a string.

---

## IsTradeAmountEnough

```lua
IsTradeAmountEnough(market, price, amount, [logWarning])
```

Gets whether or not the trade amount is high enough for exchange limits.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | Yes | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |
| `price` | `number` | Yes | The price on which to check the trade amount. |
| `amount` | `number` | Yes | The amount to check. |
| `logWarning` | `boolean` | No | If true, a warning is logged. |

**Returns:** `boolean`

Returns whether or not the trade amount is high enough for exchange limits.

---

## IsValidMarket

```lua
IsValidMarket(market)
```

Checks if the input is a valid market.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | Yes | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |

**Returns:** `boolean`

Returns true if the market is valid.

---

## MakersFee

```lua
MakersFee([market])
```

Gets makers fee of the market.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |

**Returns:** `number`

Returns makers fee of the market.

---

## MarketType

```lua
MarketType([market])
```

Gets the market type enumeration of the market.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |

**Returns:** `enum`

Returns SpotTrading, MarginTrading or LeverageTrading.

---

## MinimumTradeAmount

```lua
MinimumTradeAmount([market], [price])
```

Calculates the minimum trade amount on a certain price while keeping exchange specifications in consideration.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |
| `price` | `number` | No | The price on which to calculate the trade amount. By default the close price is used. |

**Returns:** `number`

Returns the minimum trade amount taking the decimal count, minimum amount and volume into account.

---

## ParseTradeAmount

```lua
ParseTradeAmount(market, price, amount)
```

Parses the trade amount to exchange specifications.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | Yes | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |
| `price` | `number` | Yes | The price on which to parse the trade amount. |
| `amount` | `number` | Yes | The amount to parse. |

**Returns:** `number`

Returns the trade amount adjusted for exchange specifications.

---

## ParseTradePrice

```lua
ParseTradePrice(market, price)
```

Parses the trade price to exchange specifications.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | Yes | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |
| `price` | `number` | Yes | The price on which to check the trade amount. |

**Returns:** `number`

Returns the trade price adjusted for exchange specifications.

---

## PriceDecimals

```lua
PriceDecimals([market], [price])
```

**Aliases:** `QuoteDecimals`

Returns a number of how many decimals is allowed for order prices.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |
| `price` | `number` | No | Specific price value. This can have an effect on the number of decimals. |

**Returns:** `number`

Returns the allowed decimal count.

---

## PriceStep

```lua
PriceStep([market])
```

**Aliases:** `QuoteStep`

Returns the minimum allowed price step for market.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |

**Returns:** `number`

Returns the price step size.

---

## ProfitLabel

```lua
ProfitLabel([market])
```

**Aliases:** `ProfitCurrency`

Returns the profit label. For reflection, in case the market is Bitstamp BTC/USD it will return 'USD'. When we are on BitMEX XBT/USD (XBTUSD), it will return 'XBT'.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |

**Returns:** `string`

Returns the profit label as string.

---

## QuoteCurrency

```lua
QuoteCurrency([market])
```

Returns the quote currency of the market. For reflection, in case we are on Bitstamp BTC/USD it will return 'USD'.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |

**Returns:** `string`

Returns the quote currency as string.

---

## SetFee

```lua
SetFee(percentage, [market])
```

Saves a custom fee percentage which is used in back tests and returned by Fee().

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `percentage` | `number` | Yes | Fee percentage to store. |
| `market` | `string` | No | The guid returned by AccountGuid(), InputAccount or InputAccountMarket for example. |

**Returns:** `void`

---

## TakersFee

```lua
TakersFee([market])
```

Gets takers fee of the market.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |

**Returns:** `number`

Returns takers fee of the market.

---

## TradeMarketContainer

```lua
TradeMarketContainer([market])
```

Retrieves all the information on the trade market.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |

**Returns:** `dynamic`

Returns the market information on an array.

---

## UnderlyingAsset

```lua
UnderlyingAsset([market])
```

Returns the underlying assets of the market. For reflection, in case we are on Bitstamp BTC/USD it will return 'BTC'. When we are on BitMEX XBT/USD (XBTUSD), it will return 'XBT'.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |

**Returns:** `string`

Returns the underlying assets as string.

---

## UsedMargin

```lua
UsedMargin(market, price, amount, leverage)
```

Calculates the used margin for a specific position.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | Yes | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |
| `price` | `number` | Yes | Position price. |
| `amount` | `number` | Yes | Position amount. |
| `leverage` | `number` | Yes | Leverage ratio. Use 0 for cross-margin. |

**Returns:** `number`

Returns the amount in the underlying currency value.

---


*Auto-generated from HaasScript source code.*
