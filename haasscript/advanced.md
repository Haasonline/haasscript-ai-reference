# Advanced Features

97 commands across 7 categories.

## Categories

- [Charting](#charting) (30 commands)
- [Custom Commands Helpers](#custom-commands-helpers) (8 commands)
- [Input Fields](#input-fields) (23 commands)
- [Input Settings](#input-settings) (6 commands)
- [MAchine Learning](#machine-learning) (2 commands)
- [Settings](#settings) (24 commands)
- [Social Media](#social-media) (4 commands)

---

# Charting

## ChangeColorOpacity

```lua
ChangeColorOpacity(color, opacity)
```

Converts HEX to RGBA with opacity.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `color` | `string` | Yes | Color string in format: hex (#000000), rgba(r,g,b,a) or rgb(r,g,b) |
| `opacity` | `number` | Yes | Opacity level from 0 - 100. |

**Returns:** `string`

Returns a RGBA color string.

---

## ChartAddAxisLabel

```lua
ChartAddAxisLabel(chartId, side, text, value, [color], [textColor])
```

Adds a label on the y-axis.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartId` | `number` | Yes | The chart index on which to add the label. |
| `side` | `enum` | Yes | The axis side on which to add the label. |
| `text` | `string` | Yes | The text on the label. |
| `value` | `number` | Yes | The y-axis value on which to place the label. |
| `color` | `string` | No | The label fill color. |
| `textColor` | `string` | No | The text color. |

**Returns:** `void`

---

## ChartSetAxisOptions

```lua
ChartSetAxisOptions(chartId, side, [low], [high], [visible], [type])
```

Gives control over axis settings such as low, high and visibility.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartId` | `number` | Yes | The chart index on which to apply the settings. |
| `side` | `enum` | Yes | The side of the chart on which to apply the settings. |
| `low` | `number` | No | Low prices array. |
| `high` | `number` | No | High prices array. |
| `visible` | `boolean` | No | Set axis visible/hidden. |
| `type` | `enum` | No | Set axis type. |

**Returns:** `void`

---

## ChartSetOptions

```lua
ChartSetOptions(chartId, [title], [height], [style])
```

Gives control over chart options such as title and height.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartId` | `number` | Yes | The chart index on which to apply the options. |
| `title` | `string` | No | Chart title. |
| `height` | `number` | No | Chart height. Below 1 will be treated as percentage (0.5-50%). |
| `style` | `enum` | No | Price plot style. |

**Returns:** `void`

---

## LineOptions

```lua
LineOptions([color], [style], [deco], [width], [offset], [side], [id], [behind], [ignoreOnAxis], [drawTrailingLine])
```

Creates a line options object to be used for Plot().

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `color` | `string` | No | The color of the line. |
| `style` | `enum` | No | The style of the line (Spiked, Smooth, Step and others). |
| `deco` | `enum` | No | The line decoration (Solid, Dashed, Dotted). |
| `width` | `number` | No | The width of the line. |
| `offset` | `number` | No | A positive offset will move the data points x number of candles. A negative number will move the data points back. |
| `side` | `enum` | No | Axis side to snap on. |
| `id` | `string` | No | Unique identifier. |
| `behind` | `boolean` | No | If true, the line will be plotted behind the price chart, if enabled. |
| `ignoreOnAxis` | `boolean` | No | If true, the line will not be taken into consideration when calculating the range on the y-axis. |
| `drawTrailingLine` | `boolean` | No | If true, a dotted line will be drawn from the last data point to the y-axis. |

**Returns:** `dynamic`

Creates a line options object.

---

## MarkCandle

```lua
MarkCandle(chartId, [depth])
```

Changes the color of a single candle.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartId` | `number` | Yes | The chart index on which to mark the candle. |
| `depth` | `number` | No | Amount of candles to mark. |

**Returns:** `void`

---

## Plot

```lua
Plot(chartId, lineName, value, [color/options])
```

Creates a data line on the chart. The line can be styled accordingly. A positive chartId will place the line below the main price chart. A negative index above.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartId` | `number` | Yes | Index on which to plot the line. |
| `lineName` | `string` | Yes | The name of the lines. This needs to be unique per index. |
| `value` | `number|table` | Yes | Value of the data point. |
| `color/options` | `dynamic` | No | Line options object or a color string. |

**Returns:** `string`

Returns a line guid which can be used to manipulate the style.

---

## PlotBands

```lua
PlotBands(lineGuid1, lineGuid2, fillColor)
```

Creates a band between 2 lines.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `lineGuid1` | `string` | Yes | The upper line guid returned by Plot(). |
| `lineGuid2` | `string` | Yes | The lower line guid returned by Plot(). |
| `fillColor` | `string` | Yes | Inner color of the bands. |

**Returns:** `void`

---

## PlotBars

```lua
PlotBars(lineGuid, [baseValue], [fillColor])
```

Changes a line to bars.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `lineGuid` | `string` | Yes | Line guid returned by Plot(). |
| `baseValue` | `number` | No | The value from where the bars are drawn. Default is 0. |
| `fillColor` | `string` | No | The inner color of the bars. Default is none. |

**Returns:** `void`

---

## PlotBBandsChart

```lua
PlotBBandsChart(chartIndex, name, upper, middle, [lower])
```

Takes care of the plotting side of a BBANDS (like) calculation and chart.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartIndex` | `number` | Yes | Chart index on which to plot. |
| `name` | `string` | Yes | Name of the line. |
| `upper` | `number|table` | Yes | Upper band data. |
| `middle` | `number|table` | Yes | Middle or lower band data. |
| `lower` | `number|table` | No | Lower band data. Optional, if not set only a band is plotted. |

**Returns:** `void`

---

## PlotBuySellZone

```lua
PlotBuySellZone(chartId, start, end)
```

Draws a horizontal zone with buy and sell lines on the chart.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartId` | `number` | Yes | The chart index on which to plot the data. |
| `start` | `number` | Yes | The lowest value of the zone. |
| `end` | `number` | Yes | The highest value of the zone. |

**Returns:** `void`

---

## PlotCircle

```lua
PlotCircle(lineGuid, [fillColor])
```

Changes the line into circles.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `lineGuid` | `string` | Yes | Line guid returned by Plot(). |
| `fillColor` | `string` | No | The inner color of the circles. Default is none. |

**Returns:** `void`

---

## PlotCloud

```lua
PlotCloud(lineGuid1, lineGuid2, opacity)
```

Creates a cloud with double colors between 2 lines.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `lineGuid1` | `string` | Yes | The first line guid returned by Plot(). |
| `lineGuid2` | `string` | Yes | The second line guid returned by Plot(). |
| `opacity` | `number` | Yes | The opacity of the inner cloud color. |

**Returns:** `void`

---

## PlotDoubleColor

```lua
PlotDoubleColor(lineGuid, baseValue, secondColor, [fillColor])
```

Changes the color of the line based on a base value.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `lineGuid` | `string` | Yes | Line guid returned by Plot(). |
| `baseValue` | `number` | Yes | The value where the line changes color. |
| `secondColor` | `string` | Yes | The color of the line below the base value. |
| `fillColor` | `string` | No | The inner color between the line and the base value. Default is none. |

**Returns:** `void`

---

## PlotHistogram

```lua
PlotHistogram(lineGuid, secondColor, [fillRaisingBars])
```

Change the line into a histogram.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `lineGuid` | `string` | Yes | Line guid returned by Plot(). |
| `secondColor` | `string` | Yes | The color of the negative bars. |
| `fillRaisingBars` | `boolean` | No | Fills the raising bars. Default is false. |

**Returns:** `void`

---

## PlotHistogramSignals

```lua
PlotHistogramSignals(chartIndex, name, histogram, [shortSignal], [longSignal])
```

Takes care of the plotting side of a MACD (like) calculation and chart.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartIndex` | `number` | Yes | Chart index on which to plot. |
| `name` | `string` | Yes | Name of the line. |
| `histogram` | `number|table` | Yes | Histogram data. |
| `shortSignal` | `number|table` | No | Optional, short data line. |
| `longSignal` | `number|table` | No | Optional, long data line. |

**Returns:** `void`

---

## PlotHorizontalLine

```lua
PlotHorizontalLine(chartId, name, color, value, [lineDecoration], [side], [id])
```

Draws a horizontal line on the chart.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartId` | `number` | Yes | The chart index on which to plot the data. |
| `name` | `string` | Yes | Name of the line. This needs to be unique per index. |
| `color` | `string` | Yes | The line color. |
| `value` | `number` | Yes | The y-axis value on which to place the line. |
| `lineDecoration` | `enum` | No | Line decoration style. Default is Solid. |
| `side` | `enum` | No | Axis side on which to snap the line. Default is RightAxis. |
| `id` | `string` | No | Unique identifier. |

**Returns:** `void`

---

## PlotHorizontalZone

```lua
PlotHorizontalZone(chartId, name, color, start, end, [side])
```

Draws a horizontal zone on the chart.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartId` | `number` | Yes | The chart index on which to plot the data. |
| `name` | `string` | Yes | Name of the zone. This needs to be unique per index. |
| `color` | `string` | Yes | The zone inner color. |
| `start` | `number` | Yes | The lowest value of the zone. |
| `end` | `number` | Yes | The highest value of the zone. |
| `side` | `enum` | No | Axis side on which to snap the line. Default is RightAxis. |

**Returns:** `void`

---

## PlotLineBuySellZone

```lua
PlotLineBuySellZone(chartIndex, name, source, start, end)
```

Plots the line with a buy and sell zone on the chart.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartIndex` | `number` | Yes | Chart index on which to plot. |
| `name` | `string` | Yes | Name of the line. |
| `source` | `number|table` | Yes | Value to plot. |
| `start` | `number` | Yes | The lowest value of the zone. |
| `end` | `number` | Yes | The highest value of the zone. |

**Returns:** `void`

---

## PlotPivot

```lua
PlotPivot(leftStrength, rightStrength)
```

Plots a pivot cross above or below the chart when detected.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `leftStrength` | `number` | Yes | Amount of bars before the pivot point that must be lower(Pivot Low)/higher(Pivot High) than the pivot price. |
| `rightStrength` | `number` | Yes | Amount of bars after the pivot point that must be lower(Pivot Low)/higher(Pivot High) than the pivot price. |

**Returns:** `void`

---

## PlotPrice

```lua
PlotPrice(chartId, market, [interval], [style], [upColor], [upFill], [downColor], [downFill], [markColor], [markFill])
```

Creates a price plot. Candlestick by default.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartId` | `number` | Yes | The chart index on which to plot the price data. |
| `market` | `string` | Yes | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |
| `interval` | `number` | No | The interval which to plot. |
| `style` | `enum` | No | The price plot style. |
| `upColor` | `string` | No | The outer color of the up candle. |
| `upFill` | `boolean` | No | The inner color of the up candle. |
| `downColor` | `string` | No | The outer color of the down candle. |
| `downFill` | `boolean` | No | The inner color of the down candle. |
| `markColor` | `string` | No | The outer color of the marked candle. |
| `markFill` | `boolean` | No | The inner color of the marked candle. |

**Returns:** `void`

---

## PlotShape

```lua
PlotShape([chartId], [shape], [color], [size], [aboveCandle], [text], [textColor], [offset])
```

Draws a shape above the candle or first line on the chart.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartId` | `number` | No | Index on which to plot the line. |
| `shape` | `enum` | No | The shape type. |
| `color` | `string` | No | The color of the shape. |
| `size` | `number` | No | Size of the shape. |
| `aboveCandle` | `boolean` | No | If true, the shape will be drawn above the candle else below. |
| `text` | `string` | No | Text above or below the candle or shape. |
| `textColor` | `string` | No | The color of the text. |
| `offset` | `number` | No | A positive offset will move the data points x number of candles. A negative number will move the data points back. |

**Returns:** `void`

---

## PlotShapes

```lua
PlotShapes(lineGuid, shape, [fillColor])
```

Changes the line into a specific shape.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `lineGuid` | `string` | Yes | Line guid returned by Plot(). |
| `shape` | `enum` | Yes | The shape type. |
| `fillColor` | `string` | No | The inner color of the circles. Default is none. |

**Returns:** `void`

---

## PlotSignalBar

```lua
PlotSignalBar(chartId, color)
```

Creates a small signal bar chart. A positive chartId will place the line below the main price chart. A negative index above.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartId` | `number` | Yes | Index on which to plot the line. |
| `color` | `string` | Yes | The color of the signal block. |

**Returns:** `void`

---

## PlotSignalEnum

```lua
PlotSignalEnum(chartId, signal)
```

Creates a small signal bar chart based on the signal enum. A positive chartId will place the line below the main price chart. A negative index above.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartId` | `number` | Yes | Index on which to plot the line. |
| `signal` | `enum` | Yes | The signal to plot. |

**Returns:** `enum`

Returns the input signal for continuation.

---

## PlotStackedArea

```lua
PlotStackedArea(lineGuids)
```

Creates a stacked area chart from a line guid collection.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `lineGuids` | `dynamic` | Yes | Line guid collection returned by multiple Plot()'s. |

**Returns:** `void`

---

## PlotVerticalLine

```lua
PlotVerticalLine(chartId, name, color, unix, [lineDecoration])
```

Draws a vertical line on the chart.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartId` | `number` | Yes | The chart index on which to plot the data. |
| `name` | `string` | Yes | Name of the line. This needs to be unique per index. |
| `color` | `string` | Yes | The line color. |
| `unix` | `number` | Yes | The x-axis timestamp on which to place the line. |
| `lineDecoration` | `enum` | No | Line decoration style. Default is Solid. |

**Returns:** `void`

---

## PlotVerticalZone

```lua
PlotVerticalZone(chartId, name, color, start, end)
```

Draws a vertical zone on the chart.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartId` | `number` | Yes | The chart index on which to plot the data. |
| `name` | `string` | Yes | Name of the zone. This needs to be unique per index. |
| `color` | `string` | Yes | The zone inner color. |
| `start` | `number` | Yes | The lowest value of the zone on the y-axis. |
| `end` | `number` | Yes | The highest value of the zone on the y-axis. |

**Returns:** `void`

---

## PlotVolume

```lua
PlotVolume(chartId, [upColor], [downColor], [upFill], [downFill], [side])
```

Creates volume bars. The bars can be styles accordingly. A positive chartId will place the line below the main price chart. A negative index above.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartId` | `number` | Yes | Index on which to plot the line. |
| `upColor` | `string` | No | The outer color of the up candle volume bars. |
| `downColor` | `string` | No | The outer color of the down candle volume bars. |
| `upFill` | `boolean` | No | The inner color of the up candle volume bars. |
| `downFill` | `boolean` | No | The inner color of the down candle volume bars. |
| `side` | `enum` | No | The axis side to snap. |

**Returns:** `void`

---

## SetStackedAreaOpacity

```lua
SetStackedAreaOpacity(chartId, opacity)
```

Configures the opacity for stacked area chart. A values between

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartId` | `number` | Yes | Index on which to plot the line. |
| `opacity` | `number` | Yes | Opacity level from 0 - 100. |

**Returns:** `void`

---


# Custom Commands Helpers

## DefineCommand

```lua
DefineCommand(name, description)
```

This defines the name and description of the custom command. This information will reflect in commands/block lists and help models.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `name` | `string` | Yes | The command name. Needs to be unique and a single word. No spaces or special characters are allowed beside number and '_'. |
| `description` | `string` | Yes | The description of the command. |

**Returns:** `void`

---

## DefineEasyIndicatorOutput

```lua
DefineEasyIndicatorOutput(result)
```

Defines the signal output for the custom indicator.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `result` | `enum` | Yes | The indicator result. |

**Returns:** `void`

---

## DefineEasyIndicatorParameters

```lua
DefineEasyIndicatorParameters([chartIndex])
```

Defines the chartIndex and interval parameter.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartIndex` | `number` | No | Default chart index. Default is 1. |

**Returns:** `dynamic`

Returns a list with values.

---

## DefineIntervalOptimization

```lua
DefineIntervalOptimization(interval)
```

Optimizes the custom command for back tests. Only use this if the command needs updates once every interval tick.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `interval` | `number` | Yes | The interval on which the result is based. |

**Returns:** `void`

---

## DefineOutput

```lua
DefineOutput(type, [value], [description], [outputSuggestions])
```

Defines the output type and data.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `type` | `enum` | Yes | The parameter type enum. |
| `value` | `dynamic` | No | The output value. Only required if type is not a void. |
| `description` | `string` | No | The output description. Only required if type is not a void. |
| `outputSuggestions` | `string` | No | The output suggestions. Only required if type is not a void. Separated by , . ; or : and can take category definitions. |

**Returns:** `void`

---

## DefineOutputIndex

```lua
DefineOutputIndex(index, type, name, description, [outputSuggestions])
```

Creates an index based output connection to be used in the visual editor.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `index` | `number` | Yes | The array index of this value/output. |
| `type` | `enum` | Yes | The parameter type enum. |
| `name` | `string` | Yes | The output name. |
| `description` | `string` | Yes | The output description. |
| `outputSuggestions` | `string` | No | The output suggestions. Separated by , . ; or : and can take category definitions. |

**Returns:** `void`

---

## DefineParameter

```lua
DefineParameter(type, name, description, isRequired, defaultValue, [inputSuggestions])
```

Defines the parameter for a custom command.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `type` | `enum` | Yes | The parameter type enum. |
| `name` | `string` | Yes | The input parameter name. |
| `description` | `string` | Yes | The input parameter description. |
| `isRequired` | `boolean` | Yes | If true, the command requires the parameter to be defined. |
| `defaultValue` | `dynamic` | Yes | The default and test parameter for the command. Try to use fake data if the parameter is required. |
| `inputSuggestions` | `string` | No | The input suggestions. Commands separated by , . ; or : and can take category definitions. |

**Returns:** `dynamic`

Returns the giving parameter or default value.

---

## Library

```lua
Library(arg)
```

Wrapper for custom command.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `arg` | `...dynamic` | Yes | Execution arguments. |

**Returns:** `dynamic`

Result of the custom command.

---


# Input Fields

## Input

```lua
Input(label, [defaultValue], [tooltip], [group])
```

Creates a script input field. The type is depending on the default value. It can be a number, text or boolean (checkbox) field. Without default value, it will be a text field.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `label` | `string` | Yes | The field label text. |
| `defaultValue` | `dynamic` | No | The default value for the field. |
| `tooltip` | `string` | No | The tooltip text for the field. |
| `group` | `string` | No | The group of the input field. |

**Returns:** `dynamic`

Returns the input value.

---

## InputAccount

```lua
InputAccount(label, [tooltip], [group])
```

Creates an account selection field.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `label` | `string` | Yes | The field label text. |
| `tooltip` | `string` | No | The tooltip text for the field. |
| `group` | `string` | No | The group of the input field. |

**Returns:** `string`

Return the selected account guid.

---

## InputAccountMarket

```lua
InputAccountMarket(label, [tooltip], [group])
```

Creates an account, market & leverage dropdown.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `label` | `string` | Yes | The field label text. |
| `tooltip` | `string` | No | The tooltip text for the field. |
| `group` | `string` | No | The group of the input field. |

**Returns:** `string`

Returns a combined string. For example 9c9aaf1f-469b-4eea-bca3-df61db37c549_BITFINEX_BTC_USD_.

---

## InputButton

```lua
InputButton(label, callback, [tooltip], [group])
```

Creates a button in the input field. When press in the UI, the bot will receive the action and execute the callback defined in the parameters.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `label` | `string` | Yes | The field label text. |
| `callback` | `Function` | Yes | The callback which will be executed when the button is pressed. |
| `tooltip` | `string` | No | The tooltip text for the field. |
| `group` | `string` | No | The group of the input field. |

**Returns:** `void`

---

## InputCdlTypes

```lua
InputCdlTypes(label, defaultValue, [tooltip], [group])
```

Creates a dropdown with available Candlestick Pattern (CDL) types. Can be used in combination with CDL().

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `label` | `string` | Yes | The field label text. |
| `defaultValue` | `enum` | Yes | The default value for the field. |
| `tooltip` | `string` | No | The tooltip text for the field. |
| `group` | `string` | No | The group of the input field. |

**Returns:** `enum`

Returns the selected CDL type as enum. For example ThreeCrowsType.

---

## InputConstant

```lua
InputConstant(label, defaultValue, [tooltip], [group])
```

Creates a dropdown with available constant in the category of the defaultValue. If for example SignalLong is set as default, all the signal constants will be available in the dropdown.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `label` | `string` | Yes | The field label text. |
| `defaultValue` | `enum` | Yes | The default value for the field. |
| `tooltip` | `string` | No | The tooltip text for the field. |
| `group` | `string` | No | The group of the input field. |

**Returns:** `enum`

Returns the selected constant type as constant.

---

## InputGroupHeader

```lua
InputGroupHeader(label)
```

Creates a group header, separating input sets.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `label` | `string` | Yes | The header label text. |

**Returns:** `void`

---

## InputInterval

```lua
InputInterval(label, [defaultValue], [tooltip], [group])
```

Creates an interval dropdown.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `label` | `string` | Yes | The field label text. |
| `defaultValue` | `number` | No | The default value for the field. |
| `tooltip` | `string` | No | The tooltip text for the field. |
| `group` | `string` | No | The group of the input field. |

**Returns:** `number`

Returns the selected interval in minutes.

---

## InputLrTypes

```lua
InputLrTypes(label, [defaultValue], [tooltip], [group])
```

Creates a dropdown with the available LR types.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `label` | `string` | Yes | The field label text. |
| `defaultValue` | `enum` | No | The default value for the field. |
| `tooltip` | `string` | No | The tooltip text for the field. |
| `group` | `string` | No | The group of the input field. |

**Returns:** `enum`

Returns the selected LR type as enum.

---

## InputMarket

```lua
InputMarket(label, [defaultValue], [tooltip], [group])
```

Creates a market dropdown. The market options are related to the main account selection.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `label` | `string` | Yes | The field label text. |
| `defaultValue` | `string` | No | The default value for the field. |
| `tooltip` | `string` | No | The tooltip text for the field. |
| `group` | `string` | No | The group of the input field. |

**Returns:** `string`

Returns the selected market. For example BITMEX_XBT_USD_XBTUSD.

---

## InputMaTypes

```lua
InputMaTypes(label, defaultValue, [tooltip], [group])
```

Creates a dropdown with available MA types. Can be used in combination with a variety of technical indicators like BBANDS() or PPO().

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `label` | `string` | Yes | The field label text. |
| `defaultValue` | `enum` | Yes | The default value for the field. |
| `tooltip` | `string` | No | The tooltip text for the field. |
| `group` | `string` | No | The group of the input field. |

**Returns:** `enum`

Returns the selected ma type as constant.

---

## InputOptions

```lua
InputOptions(label, defaultValue, options, [tooltip], [group])
```

Creates a dropdown with custom options.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `label` | `string` | Yes | The field label text. |
| `defaultValue` | `string` | Yes | The default value for the field. |
| `options` | `dynamic` | Yes | Options list/array. |
| `tooltip` | `string` | No | The tooltip text for the field. |
| `group` | `string` | No | The group of the input field. |

**Returns:** `string`

Returns the selected value as string.

---

## InputOrderType

```lua
InputOrderType(label, [defaultValue], [tooltip], [group])
```

Creates a dropdown with the default available order types.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `label` | `string` | Yes | The field label text. |
| `defaultValue` | `enum` | No | The default value for the field. |
| `tooltip` | `string` | No | The tooltip text for the field. |
| `group` | `string` | No | The group of the input field. |

**Returns:** `enum`

Returns the selected order type as enum.

---

## InputPriceSource

```lua
InputPriceSource(label, [defaultValue], [tooltip], [group])
```

Creates a price source dropdown.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `label` | `string` | Yes | The field label text. |
| `defaultValue` | `string` | No | The default value for the field. |
| `tooltip` | `string` | No | The tooltip text for the field. |
| `group` | `string` | No | The group of the input field. |

**Returns:** `string`

Returns the selected price source as string. For example BITSTAMP.

---

## InputPriceSourceMarket

```lua
InputPriceSourceMarket(label, [defaultValue], [type], [tooltip], [group])
```

Creates a price source & market dropdown.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `label` | `string` | Yes | The field label text. |
| `defaultValue` | `string` | No | The default value for the field. |
| `type` | `enum` | No | Optional parameter to restrict market types. |
| `tooltip` | `string` | No | The tooltip text for the field. |
| `group` | `string` | No | The group of the input field. |

**Returns:** `string`

Returns the selected market as string. For example BITSTAMP_BTC_USD_.

---

## InputSignalManagement

```lua
InputSignalManagement(label, signal, [tooltip], [group])
```

Creates multiple dropdown options for signal management and mapping.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `label` | `string` | Yes | The field label text. |
| `signal` | `enum` | Yes | The default value for the field. |
| `tooltip` | `string` | No | The tooltip text for the field. |
| `group` | `string` | No | The group of the input field. |

**Returns:** `enum`

Returns the baked signal based on settings.

---

## InputSignalTypes

```lua
InputSignalTypes(label, defaultValue, [tooltip], [group])
```

Creates a dropdown with available Signal Types. Can be used in combination with GetSuperSignal().

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `label` | `string` | Yes | The field label text. |
| `defaultValue` | `enum` | Yes | The default value for the field. |
| `tooltip` | `string` | No | The tooltip text for the field. |
| `group` | `string` | No | The group of the input field. |

**Returns:** `enum`

Returns the selected signal type enum.

---

## InputSlider

```lua
InputSlider(label, [min], [max], [steps], [tooltip], [group])
```

Creates a script input slider.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `label` | `string` | Yes | The field label text. |
| `min` | `number` | No | The minimum value of the slider. Default is zero. |
| `max` | `number` | No | The maximum value of the slider. Default is one. |
| `steps` | `number` | No | The step count of the slider. Default is 10. |
| `tooltip` | `string` | No | The tooltip text for the field. |
| `group` | `string` | No | The group of the input field. |

**Returns:** `number`

Returns the slider value.

---

## InputSourcePrice

```lua
InputSourcePrice(label, defaultValue, [tooltip], [group])
```

Creates a dropdown with available Source Price types. Can be used in combination with SourcePrices().

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `label` | `string` | Yes | The field label text. |
| `defaultValue` | `enum` | Yes | The default value for the field. |
| `tooltip` | `string` | No | The tooltip text for the field. |
| `group` | `string` | No | The group of the input field. |

**Returns:** `enum`

Returns the selected source type as constant.

---

## InputTable

```lua
InputTable(options, columns)
```

Creates a dynamic input table.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `options` | `dynamic` | Yes | InputTableOptions() result or the title of the table as a string. |
| `columns` | `...dynamic` | Yes | InputTableColumn() or columns headers as string. |

**Returns:** `dynamic`

Returns the tables values per row. [[1,2,3,4], [5,6,7,8], [9,0,1,2]].

---

## InputTableColumn

```lua
InputTableColumn(name, [values[]])
```

Creates an table column options object for InputTable()

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `name` | `string` | Yes | Column name. |
| `values[]` | `...dynamic` | No | Default values. |

**Returns:** `dynamic`

Returns the options objects.

---

## InputTableColumnDropdown

```lua
InputTableColumnDropdown(name, options, [values[]])
```

Creates an table column options object for InputTable() with a dropdown

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `name` | `string` | Yes | Column name. |
| `options` | `dynamic` | Yes | Dropdown options. For example { "options-A", "options-B" } |
| `values[]` | `...dynamic` | No | Default values. |

**Returns:** `dynamic`

Returns the options objects.

---

## InputTableOptions

```lua
InputTableOptions([title], [rows], [maxRows], [group])
```

Creates an options object for InputTable().

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `title` | `string` | No | Name of the table. |
| `rows` | `number` | No | Fixed number of rows. Overwrites maxRows. |
| `maxRows` | `number` | No | Dynamic number of rows with a maximum. |
| `group` | `string` | No | Group of the table. |

**Returns:** `dynamic`

Returns the options objects.

---


# Input Settings

## AccountGuid

```lua
AccountGuid()
```

Gets the main account guid of the bot.

**Returns:** `string`

Returns an account guid.

---

## CurrentInterval

```lua
CurrentInterval([sources])
```

Gets the default interval selected by the user or returns the interval of a price source.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `sources` | `...dynamic` | No | Source data. |

**Returns:** `number`

Interval in minutes.

---

## Fee

```lua
Fee([market])
```

Gets the current fee percentage of the main or specific market.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The guid returned by AccountGuid(), InputAccount or InputAccountMarket for example. |

**Returns:** `number`

Returns the fee percentage.

---

## Leverage

```lua
Leverage()
```

Gets the main account leverage setting.

**Returns:** `number`

Leverage setting.

---

## PriceMarket

```lua
PriceMarket()
```

Gets the main market of the bot.

**Returns:** `string`

Returns a market string. For example: BITFINEX_BTC_USD_ or BITMEX_XBT_USD_XBTUSD.

---

## TradeAmount

```lua
TradeAmount()
```

Gets the trade amount setting.

**Returns:** `number`

Gets the trade amount.

---


# MAchine Learning

## IsAbnormal

```lua
IsAbnormal(dataset, multiplier)
```

Looks to a numberic array and tells if the last value is abnormal

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `dataset` | `HaasNumberCollection` | Yes | Dataset |
| `multiplier` | `number` | Yes | Multiplier to control the sensitivity (1-3) |

**Returns:** `boolean`

---

## SimpleForecastBySsa

```lua
SimpleForecastBySsa(dataset, L, N, forecastCount)
```

Reads the given data and tries to give a forecast

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `dataset` | `HaasNumberCollection` | Yes | Dataset |
| `L` | `number` | Yes | Length of the window on the series for building the trajectory matrix (L) |
| `N` | `number` | Yes | Length of series that is kept in buffer for modeling (N) |
| `forecastCount` | `number` | Yes | Amount of future data points |

**Returns:** `HaasNumberCollection`

---


# Settings

## DeactivateBot

```lua
DeactivateBot([reason], [cancelOpenOrders])
```

Deactivates the bot. Once the bot is deactivated, it can only manually activate again.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `reason` | `string` | No | Reason for deactivation. |
| `cancelOpenOrders` | `boolean` | No | If set, all open orders of the bot are being canceled. |

**Returns:** `void`

---

## DisableIndicatorContainerLogs

```lua
DisableIndicatorContainerLogs()
```

Disables IndicatorContainer log messages.

**Returns:** `void`

---

## EnableHighSpeedUpdates

```lua
EnableHighSpeedUpdates([updateOnFilledOrders])
```

Enables high-speed script execution.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `updateOnFilledOrders` | `boolean` | No | Executes an update right after an order has finished (canceled or filled). |

**Returns:** `void`

---

## EnableOrderPersistence

```lua
EnableOrderPersistence()
```

Enables order persistence for managed trading.

**Returns:** `void`

---

## Finalize

```lua
Finalize([callback])
```

Only executes on the last update cycle of a backtest. Saving time in the overall backtest length. Can be used in combination with plot manipulation (eg. PlotDoubleColor()) or CustomReport()

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `callback` | `dynamic` | No | The function to executed when a candle closes. |

**Returns:** `boolean`

Returns true when executed.

---

## GetHaasScriptVersion

```lua
GetHaasScriptVersion()
```

Gets the current HaasScript version as a number.

**Returns:** `number`

Returns the version number.

---

## GetLeverage

```lua
GetLeverage([market], [accountId])
```

Gets the set leverage for current or specified market.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |
| `accountId` | `string` | No | The guid returned by AccountGuid(), InputAccount or InputAccountMarket for example. |

**Returns:** `number`

Returns the leverage value or -1 if request failed.

---

## GetMarginMode

```lua
GetMarginMode([market], [accountId])
```

Gets the current margin mode for current or specified market.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |
| `accountId` | `string` | No | The guid returned by AccountGuid(), InputAccount or InputAccountMarket for example. |

**Returns:** `enum`

Returns the margin mode enum.

---

## GetMaxLeverage

```lua
GetMaxLeverage([market])
```

Gets the maximum leverage for current or specified market.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |

**Returns:** `number`

Returns the maximum leverage value or -1 if request failed.

---

## GetOrderType

```lua
GetOrderType()
```

Returns the current main order type.

**Returns:** `enum`

Returns the order type enum.

---

## GetPositionMode

```lua
GetPositionMode([accountId], [market])
```

Gets the current position mode for current or specified market.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `accountId` | `string` | No | The guid returned by AccountGuid(), InputAccount or InputAccountMarket for example. |
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |

**Returns:** `enum`

Returns the position mode enum.

---

## HideOrderSettings

```lua
HideOrderSettings()
```

Hides the 2 order settings fields.

**Returns:** `void`

---

## HideTradeAmountSettings

```lua
HideTradeAmountSettings()
```

Hides the 2 trade amount settings fields.

**Returns:** `void`

---

## IsBotPaused

```lua
IsBotPaused()
```

Used in combination with Pause- & ResumeBot(). The value can also be updated manually though the UI.

**Returns:** `boolean`

Returns true if the pause flag is set.

---

## IsEnterpriseVersion

```lua
IsEnterpriseVersion()
```

Returns true if running the enterprise version. Otherwise false.

**Returns:** `boolean`

Returns a boolean.

---

## IsMarginModeSupported

```lua
IsMarginModeSupported(marginMode, [accountId])
```

Checks if the specified margin mode is supported on current or specified market.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `marginMode` | `enum` | Yes | Request margin mode. CrossMarginMode or IsolatedMarginMode |
| `accountId` | `string` | No | The guid returned by AccountGuid(), InputAccount or InputAccountMarket for example. |

**Returns:** `boolean`

Returns true if supported, otherwise false.

---

## IsPositionModeSupported

```lua
IsPositionModeSupported(positionMode, [accountId])
```

Checks if specified position mode is supported on current or specified market.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `positionMode` | `enum` | Yes | Request position mode. OneWayPositionMode or HedgePositionMode |
| `accountId` | `string` | No | The guid returned by AccountGuid(), InputAccount or InputAccountMarket for example. |

**Returns:** `boolean`

Returns true if supported, otherwise false.

---

## OptimizedForInterval

```lua
OptimizedForInterval(interval, callback)
```

Executes and saves or loads a value depending on the timestamp and interval. This command can be used to speed up backtest calculations which are depending on interval based data. For examples indicator calculation with closed candles.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `interval` | `number` | Yes | The interval on which the execution is based. If set on 0, the main interval is used. |
| `callback` | `dynamic` | Yes | The function to executed when a candle closes. |

**Returns:** `dynamic`

Returns the saved value or result of the callback.

---

## PauseBot

```lua
PauseBot()
```

Enables the paused flag. Use in combination with IsBotPaused() to read the value. Can be used to temporary block certain actions like increasing position without stopping the whole bot. Bots will continue to update and are allowed to place/cancel orders.

**Returns:** `void`

---

## ResumeBot

```lua
ResumeBot()
```

Disables the pause flag. Use in combination with IsBotPaused() to read the value

**Returns:** `void`

---

## SetLeverage

```lua
SetLeverage(leverage, [market], [accountId])
```

Sets the leverage ratio for current or specified market.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `leverage` | `number` | Yes | The amount of leverage to set. |
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |
| `accountId` | `string` | No | The guid returned by AccountGuid(), InputAccount or InputAccountMarket for example. |

**Returns:** `boolean`

Returns true if successfully set value, otherwise false.

---

## SetMarginMode

```lua
SetMarginMode(marginMode, [market], [accountId])
```

Sets the margin mode for current or specific market. It is NOT possible to change margin mode while having an open position.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `marginMode` | `enum` | Yes | Request margin mode. CrossMarginMode or IsolatedMarginMode |
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |
| `accountId` | `string` | No | The guid returned by AccountGuid(), InputAccount or InputAccountMarket for example. |

**Returns:** `boolean`

Returns true if successfully set, otherwise false.

---

## SetOrderType

```lua
SetOrderType(type)
```

Overwrites the order type with the given type. This can be used to execute different order types on managed trading.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `type` | `enum` | Yes | Order type enum. |

**Returns:** `void`

---

## SetPositionMode

```lua
SetPositionMode(positionMode, [accountId], [market])
```

Sets the position mode for current or specified market. It is NOT possible to change position mode while having an open position.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `positionMode` | `enum` | Yes | Request position mode. OneWayPositionMode or HedgePositionMode |
| `accountId` | `string` | No | The guid returned by AccountGuid(), InputAccount or InputAccountMarket for example. |
| `market` | `string` | No | The market returned by PriceMarket(), InputAccountMarket() or InputMarket() for example. |

**Returns:** `boolean`

Returns true if successfully set, otherwise false.

---


# Social Media

## BroadcastToDiscord

```lua
BroadcastToDiscord(message)
```

Send a text message to Discord.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `message` | `string` | Yes | The content of the message |

**Returns:** `void`

---

## BroadcastToSlack

```lua
BroadcastToSlack(message)
```

Send a text message to Slack.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `message` | `string` | Yes | The content of the message |

**Returns:** `void`

---

## BroadcastToTelegram

```lua
BroadcastToTelegram(message)
```

Send a text message to your Telegram.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `message` | `string` | Yes | The content of the message |

**Returns:** `void`

---

## BroadcastToTwitter

```lua
BroadcastToTwitter(message)
```

Sends a new post to your twitter timeline.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `message` | `string` | Yes | The content of the message |

**Returns:** `void`

---


*Auto-generated from HaasScript source code.*
