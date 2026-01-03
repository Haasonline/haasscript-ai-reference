# Technical Analysis

157 commands across 3 categories.

## Categories

- [Easy Indicators](#easy-indicators) (50 commands)
- [Technical Analysis](#technical-analysis) (98 commands)
- [Technical Analysis Helpers](#technical-analysis-helpers) (9 commands)

---

# Easy Indicators

## EasyABANDS

```lua
EasyABANDS(chartIndex, [name], [interval])
```

Automatically defines the input fields, calculates the ABANDS, plots the chart and returns the signal.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartIndex` | `number` | Yes | Chart index on which to plot. |
| `name` | `string` | No | Unique name of the indicator. |
| `interval` | `number` | No | Used interval for price data. Default is 0 and the main interval will be used. |

**Returns:** `enum`

Returns a signal based on the GetBuySellLevelSignal() template. Signals are generated when price breaks above Upper and below Lower band.

---

## EasyADOSC

```lua
EasyADOSC(chartIndex, [name], [interval])
```

Automatically defines the input fields, calculates the ADOSC, plots the chart and returns the signal.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartIndex` | `number` | Yes | Chart index on which to plot. |
| `name` | `string` | No | Unique name of the indicator. |
| `interval` | `number` | No | Used interval for price data. Default is 0 and the main interval will be used. |

**Returns:** `enum`

Returns a signal based on ADOSC, it's MA and selected signal type.

---

## EasyAlice

```lua
EasyAlice([interval])
```

Automatically defines the input fields, calculates the Alice signal based on the Haasonline Intelli Alice bot. By default Alice works on a 2 minute interval., plots the chart and returns the signal.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `interval` | `number` | No | Used interval for price data. Default is 0 and the main interval will be used. |

**Returns:** `enum`

Returns a signal based on the Intelli-Bot Alice.

---

## EasyAO

```lua
EasyAO(chartIndex, [name], [interval])
```

Automatically defines the input fields, calculates the AO, plots the chart and returns the signal.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartIndex` | `number` | Yes | Chart index on which to plot. |
| `name` | `string` | No | Unique name of the indicator. |
| `interval` | `number` | No | Used interval for price data. Default is 0 and the main interval will be used. |

**Returns:** `enum`

Returns a signal based on AO, it's MA and selected signal type.

---

## EasyAPO

```lua
EasyAPO(chartIndex, [name], [interval])
```

Automatically defines the input fields, calculates the APO, plots the chart and returns the signal.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartIndex` | `number` | Yes | Chart index on which to plot. |
| `name` | `string` | No | Unique name of the indicator. |
| `interval` | `number` | No | Used interval for price data. Default is 0 and the main interval will be used. |

**Returns:** `enum`

Returns a signal based on APO, it's MA and selected signal type.

---

## EasyAROON

```lua
EasyAROON(chartIndex, [name], [interval])
```

Automatically defines the input fields, calculates the AROON, plots the chart and returns the signal.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartIndex` | `number` | Yes | Chart index on which to plot. |
| `name` | `string` | No | Unique name of the indicator. |
| `interval` | `number` | No | Used interval for price data. Default is 0 and the main interval will be used. |

**Returns:** `enum`

Returns a signal based on the GetThresholdSignal() template.

---

## EasyAROONOSC

```lua
EasyAROONOSC(chartIndex, [name], [interval])
```

Automatically defines the input fields, calculates the AROONOSC, plots the chart and returns the signal.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartIndex` | `number` | Yes | Chart index on which to plot. |
| `name` | `string` | No | Unique name of the indicator. |
| `interval` | `number` | No | Used interval for price data. Default is 0 and the main interval will be used. |

**Returns:** `enum`

Returns a signal based on the GetThresholdSignal() template.

---

## EasyBBANDS

```lua
EasyBBANDS(chartIndex, [name], [interval])
```

Automatically defines the input fields, calculates the BBANDS, plots the chart and returns the signal.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartIndex` | `number` | Yes | Chart index on which to plot. |
| `name` | `string` | No | Unique name of the indicator. |
| `interval` | `number` | No | Used interval for price data. Default is 0 and the main interval will be used. |

**Returns:** `enum`

Returns a signal based on the GetBuySellLevelSignal() template. Signals are generated when price breaks above Upper and below Lower band.

---

## EasyBBANDSB

```lua
EasyBBANDSB(chartIndex, [name], [interval])
```

Automatically defines the input fields, calculates the BBANDS %B, plots the chart and returns the signal.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartIndex` | `number` | Yes | Chart index on which to plot. |
| `name` | `string` | No | Unique name of the indicator. |
| `interval` | `number` | No | Used interval for price data. Default is 0 and the main interval will be used. |

**Returns:** `enum`

Returns a signal based on the GetBuySellLevelSignal() template. Signals are generated when price breaks above Upper and below Lower band.

---

## EasyBBANDSW

```lua
EasyBBANDSW(chartIndex, [name], [interval])
```

Automatically defines the input fields, calculates the BBANDS %W, plots the chart and returns the signal.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartIndex` | `number` | Yes | Chart index on which to plot. |
| `name` | `string` | No | Unique name of the indicator. |
| `interval` | `number` | No | Used interval for price data. Default is 0 and the main interval will be used. |

**Returns:** `enum`

Returns a signal based on the GetBuySellLevelSignal() template. Signals are generated when price breaks above Upper and below Lower band.

---

## EasyBOP

```lua
EasyBOP(chartIndex, [name], [interval])
```

Automatically defines the input fields, calculates the BOP, plots the chart and returns the signal.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartIndex` | `number` | Yes | Chart index on which to plot. |
| `name` | `string` | No | Unique name of the indicator. |
| `interval` | `number` | No | Used interval for price data. Default is 0 and the main interval will be used. |

**Returns:** `enum`

Returns a signal based on BOP, it's MA and selected signal type.

---

## EasyCCI

```lua
EasyCCI(chartIndex, [name], [interval])
```

Automatically defines the input fields, calculates the CCI, plots the chart and returns the signal.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartIndex` | `number` | Yes | Chart index on which to plot. |
| `name` | `string` | No | Unique name of the indicator. |
| `interval` | `number` | No | Used interval for price data. Default is 0 and the main interval will be used. |

**Returns:** `enum`

Returns a signal based on CCI, it's MA and selected signal type.

---

## EasyCDL

```lua
EasyCDL(chartIndex, [name], [interval])
```

Automatically defines the input fields, calculates the CDL, plots the chart and returns the signal.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartIndex` | `number` | Yes | Chart index on which to plot. |
| `name` | `string` | No | Unique name of the indicator. |
| `interval` | `number` | No | Used interval for price data. Default is 0 and the main interval will be used. |

**Returns:** `enum`

Returns a signal based on selected Candle Pattern.

---

## EasyCMO

```lua
EasyCMO(chartIndex, [name], [interval])
```

Automatically defines the input fields, calculates the CMO, plots the chart and returns the signal.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartIndex` | `number` | Yes | Chart index on which to plot. |
| `name` | `string` | No | Unique name of the indicator. |
| `interval` | `number` | No | Used interval for price data. Default is 0 and the main interval will be used. |

**Returns:** `enum`

Returns a signal based on CMO, it's MA and selected signal type.

---

## EasyCOPPOCK

```lua
EasyCOPPOCK(chartIndex, [name], [interval])
```

Automatically defines the input fields, calculates the COPPOCK, plots the chart and returns the signal.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartIndex` | `number` | Yes | Chart index on which to plot. |
| `name` | `string` | No | Unique name of the indicator. |
| `interval` | `number` | No | Used interval for price data. Default is 0 and the main interval will be used. |

**Returns:** `enum`

Returns a signal based on COPPOCK, it's MA and selected signal type.

---

## EasyCRSI

```lua
EasyCRSI(chartIndex, [name], [interval])
```

Automatically defines the input fields, calculates the CRSI, plots the chart and returns the signal.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartIndex` | `number` | Yes | Chart index on which to plot. |
| `name` | `string` | No | Unique name of the indicator. |
| `interval` | `number` | No | Used interval for price data. Default is 0 and the main interval will be used. |

**Returns:** `enum`

Returns a signal based on the GetBuySellLevelSignal() template.

---

## EasyDMI

```lua
EasyDMI(chartIndex, [name], [interval])
```

Automatically defines the input fields, calculates the ADX, +DI, -DI, plots the chart and returns the signal.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartIndex` | `number` | Yes | Chart index on which to plot. |
| `name` | `string` | No | Unique name of the indicator. |
| `interval` | `number` | No | Used interval for price data. Default is 0 and the main interval will be used. |

**Returns:** `enum`

Returns a signal based on ADX level and +DI/-DI crossover.

---

## EasyDONCHIAN

```lua
EasyDONCHIAN(chartIndex, [name], [interval])
```

Automatically defines the input fields, calculates the DONCHIAN, plots the chart and returns the signal.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartIndex` | `number` | Yes | Chart index on which to plot. |
| `name` | `string` | No | Unique name of the indicator. |
| `interval` | `number` | No | Used interval for price data. Default is 0 and the main interval will be used. |

**Returns:** `enum`

Returns a signal based on the GetBuySellLevelSignal() template. Signals are produced when price breaks above upper or below lower band.

---

## EasyDPO

```lua
EasyDPO(chartIndex, [name], [interval])
```

Automatically defines the input fields, calculates the DPO, plots the chart and returns the signal.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartIndex` | `number` | Yes | Chart index on which to plot. |
| `name` | `string` | No | Unique name of the indicator. |
| `interval` | `number` | No | Used interval for price data. Default is 0 and the main interval will be used. |

**Returns:** `enum`

Returns a signal based on DPO, it's MA and selected signal type.

---

## EasyDX

```lua
EasyDX(chartIndex, [name], [interval])
```

Automatically defines the input fields, calculates the DX, plots the chart and returns the signal.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartIndex` | `number` | Yes | Chart index on which to plot. |
| `name` | `string` | No | Unique name of the indicator. |
| `interval` | `number` | No | Used interval for price data. Default is 0 and the main interval will be used. |

**Returns:** `enum`

Returns a signal based on the GetThresholdSignal() template.

---

## EasyDynamicLongShortLevels

```lua
EasyDynamicLongShortLevels(chartIndex, [name], [interval])
```

Automatically defines the input fields, calculates the DynamicBuySellLevel, plots the chart and returns the signal.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartIndex` | `number` | Yes | Chart index on which to plot. |
| `name` | `string` | No | Unique name of the indicator. |
| `interval` | `number` | No | Used interval for price data. Default is 0 and the main interval will be used. |

**Returns:** `enum`

Returns a signal based on dynamic short/long level.

---

## EasyFastRSI

```lua
EasyFastRSI(chartIndex, [name], [interval])
```

Automatically defines the input fields, calculates the FastRSI, plots the chart and returns the signal.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartIndex` | `number` | Yes | Chart index on which to plot. |
| `name` | `string` | No | Unique name of the indicator. |
| `interval` | `number` | No | Used interval for price data. Default is 0 and the main interval will be used. |

**Returns:** `enum`

Returns a signal based on FastRSI & RSI values and selected signal type.

---

## EasyFIBONACCI

```lua
EasyFIBONACCI(chartIndex, [name], [interval])
```

Automatically defines the input fields, calculates the FIBONACCI, plots the chart and returns the signal.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartIndex` | `number` | Yes | Chart index on which to plot. |
| `name` | `string` | No | Unique name of the indicator. |
| `interval` | `number` | No | Used interval for price data. Default is 0 and the main interval will be used. |

**Returns:** `enum`

Returns a signal based on FIBONACCI.

---

## EasyFixedLongShortLevels

```lua
EasyFixedLongShortLevels(chartIndex, [name], [interval])
```

Automatically defines the input fields, calculates the FixedLongShort, plots the chart and returns the signal.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartIndex` | `number` | Yes | Chart index on which to plot. |
| `name` | `string` | No | Unique name of the indicator. |
| `interval` | `number` | No | Used interval for price data. Default is 0 and the main interval will be used. |

**Returns:** `enum`

Returns a signal based on fixed long/short levels.

---

## EasyICHIMOKU

```lua
EasyICHIMOKU(chartIndex, [name], [interval])
```

Automatically defines the input fields, calculates the ICHIMOKU, plots the chart and returns the signal.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartIndex` | `number` | Yes | Chart index on which to plot. |
| `name` | `string` | No | Unique name of the indicator. |
| `interval` | `number` | No | Used interval for price data. Default is 0 and the main interval will be used. |

**Returns:** `enum`

Returns a signal based on ICHIMOKU and selected signal type.

---

## EasyIMI

```lua
EasyIMI(chartIndex, [name], [interval])
```

Automatically defines the input fields, calculates the IMI, plots the chart and returns the signal.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartIndex` | `number` | Yes | Chart index on which to plot. |
| `name` | `string` | No | Unique name of the indicator. |
| `interval` | `number` | No | Used interval for price data. Default is 0 and the main interval will be used. |

**Returns:** `enum`

Returns a signal based on IMI, it's MA and selected signal type.

---

## EasyKELTNER

```lua
EasyKELTNER(chartIndex, [name], [interval])
```

Automatically defines the input fields, calculates the KELTNER, plots the chart and returns the signal.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartIndex` | `number` | Yes | Chart index on which to plot. |
| `name` | `string` | No | Unique name of the indicator. |
| `interval` | `number` | No | Used interval for price data. Default is 0 and the main interval will be used. |

**Returns:** `enum`

Returns a signal based on the GetBuySellLevelSignal() template. Signals are produced when price breaks above upper or below lower band.

---

## EasyKRI

```lua
EasyKRI(chartIndex, [name], [interval])
```

Automatically defines the input fields, calculates the KRI, plots the chart and returns the signal.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartIndex` | `number` | Yes | Chart index on which to plot. |
| `name` | `string` | No | Unique name of the indicator. |
| `interval` | `number` | No | Used interval for price data. Default is 0 and the main interval will be used. |

**Returns:** `enum`

Returns a signal based on KRI, it's MA and selected signal type.

---

## EasyKST

```lua
EasyKST(chartIndex, [name], [interval])
```

Automatically defines the input fields, calculates the KST, plots the chart and returns the signal.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartIndex` | `number` | Yes | Chart index on which to plot. |
| `name` | `string` | No | Unique name of the indicator. |
| `interval` | `number` | No | Used interval for price data. Default is 0 and the main interval will be used. |

**Returns:** `enum`

Returns a signal based on KST and selected signal type.

---

## EasyLINEARREG

```lua
EasyLINEARREG(chartIndex, [name], [interval])
```

Automatically defines the input fields, calculates the LINEARREG, plots the chart and returns the signal.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartIndex` | `number` | Yes | Chart index on which to plot. |
| `name` | `string` | No | Unique name of the indicator. |
| `interval` | `number` | No | Used interval for price data. Default is 0 and the main interval will be used. |

**Returns:** `enum`

Returns a signal based on the GetAboveBelowSignal() template.

---

## EasyMA

```lua
EasyMA(chartIndex, [name], [interval])
```

Automatically defines the input fields, calculates the MA, plots the chart and returns the signal.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartIndex` | `number` | Yes | Chart index on which to plot. |
| `name` | `string` | No | Unique name of the indicator. |
| `interval` | `number` | No | Used interval for price data. Default is 0 and the main interval will be used. |

**Returns:** `enum`

Returns a signal based on the GetAboveBelowSignal() template. Signals are produced when faster MA crosses above or below the slower MA.

---

## EasyMACD

```lua
EasyMACD(chartIndex, [name], [interval])
```

Automatically defines the input fields, calculates the MACD, plots the chart and returns the signal.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartIndex` | `number` | Yes | Chart index on which to plot. |
| `name` | `string` | No | Unique name of the indicator. |
| `interval` | `number` | No | Used interval for price data. Default is 0 and the main interval will be used. |

**Returns:** `enum`

Returns a signal based on MACD and selected signal type.

---

## EasyMFI

```lua
EasyMFI(chartIndex, [name], [interval])
```

Automatically defines the input fields, calculates the MFI, plots the chart and returns the signal.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartIndex` | `number` | Yes | Chart index on which to plot. |
| `name` | `string` | No | Unique name of the indicator. |
| `interval` | `number` | No | Used interval for price data. Default is 0 and the main interval will be used. |

**Returns:** `enum`

Returns a signal based on MFI, it's MA and selected signal type.

---

## EasyMOM

```lua
EasyMOM(chartIndex, [name], [interval])
```

Automatically defines the input fields, calculates the MOM, plots the chart and returns the signal.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartIndex` | `number` | Yes | Chart index on which to plot. |
| `name` | `string` | No | Unique name of the indicator. |
| `interval` | `number` | No | Used interval for price data. Default is 0 and the main interval will be used. |

**Returns:** `enum`

Returns a signal based on MOM, it's MA and selected signal type.

---

## EasyOBV

```lua
EasyOBV(chartIndex, [name], [interval])
```

Automatically defines the input fields, calculates the OBV, plots the chart and returns the signal.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartIndex` | `number` | Yes | Chart index on which to plot. |
| `name` | `string` | No | Unique name of the indicator. |
| `interval` | `number` | No | Used interval for price data. Default is 0 and the main interval will be used. |

**Returns:** `enum`

Returns a signal based on OBV, it's MA and selected signal type.

---

## EasyPPO

```lua
EasyPPO(chartIndex, [name], [interval])
```

Automatically defines the input fields, calculates the PPO, plots the chart and returns the signal.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartIndex` | `number` | Yes | Chart index on which to plot. |
| `name` | `string` | No | Unique name of the indicator. |
| `interval` | `number` | No | Used interval for price data. Default is 0 and the main interval will be used. |

**Returns:** `enum`

Returns a signal based on PPO and selected signal type.

---

## EasyROC

```lua
EasyROC(chartIndex, [name], [interval])
```

Automatically defines the input fields, calculates the ROC, plots the chart and returns the signal.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartIndex` | `number` | Yes | Chart index on which to plot. |
| `name` | `string` | No | Unique name of the indicator. |
| `interval` | `number` | No | Used interval for price data. Default is 0 and the main interval will be used. |

**Returns:** `enum`

Returns a signal based on ROC, it's MA and selected signal type.

---

## EasyRSI

```lua
EasyRSI(chartIndex, [name], [interval])
```

Automatically defines the input fields, calculates the RSI, plots the chart and returns the signal.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartIndex` | `number` | Yes | Chart index on which to plot. |
| `name` | `string` | No | Unique name of the indicator. |
| `interval` | `number` | No | Used interval for price data. Default is 0 and the main interval will be used. |

**Returns:** `enum`

Returns a signal based on the GetBuySellLevelSignal() template.

---

## EasySAR

```lua
EasySAR(chartIndex, [name], [interval])
```

Automatically defines the input fields, calculates the SAR, plots the chart and returns the signal.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartIndex` | `number` | Yes | Chart index on which to plot. |
| `name` | `string` | No | Unique name of the indicator. |
| `interval` | `number` | No | Used interval for price data. Default is 0 and the main interval will be used. |

**Returns:** `enum`

Returns a signal based on SAR.

---

## EasySlowRSI

```lua
EasySlowRSI(chartIndex, [name], [interval])
```

Automatically defines the input fields, calculates the SlowRSI, plots the chart and returns the signal.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartIndex` | `number` | Yes | Chart index on which to plot. |
| `name` | `string` | No | Unique name of the indicator. |
| `interval` | `number` | No | Used interval for price data. Default is 0 and the main interval will be used. |

**Returns:** `enum`

Returns a signal based on SlowRSI & RSI values and selected signal type.

---

## EasySSTOCH

```lua
EasySSTOCH(chartIndex, [name], [interval])
```

Automatically defines the input fields, calculates the SSTOCH, plots the chart and returns the signal.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartIndex` | `number` | Yes | Chart index on which to plot. |
| `name` | `string` | No | Unique name of the indicator. |
| `interval` | `number` | No | Used interval for price data. Default is 0 and the main interval will be used. |

**Returns:** `enum`

Returns a signal based on the GetBuySellLevelSignal() template.

---

## EasySTOCH

```lua
EasySTOCH(chartIndex, [name], [interval])
```

Automatically defines the input fields, calculates the STOCH, plots the chart and returns the signal.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartIndex` | `number` | Yes | Chart index on which to plot. |
| `name` | `string` | No | Unique name of the indicator. |
| `interval` | `number` | No | Used interval for price data. Default is 0 and the main interval will be used. |

**Returns:** `enum`

Returns a signal based on %K, %D and selected signal type.

---

## EasySTOCHF

```lua
EasySTOCHF(chartIndex, [name], [interval])
```

Automatically defines the input fields, calculates the STOCHF, plots the chart and returns the signal.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartIndex` | `number` | Yes | Chart index on which to plot. |
| `name` | `string` | No | Unique name of the indicator. |
| `interval` | `number` | No | Used interval for price data. Default is 0 and the main interval will be used. |

**Returns:** `enum`

Returns a signal based on the GetCrossOverUnderSignal() template.

---

## EasySTOCHRSI

```lua
EasySTOCHRSI(chartIndex, [name], [interval])
```

Automatically defines the input fields, calculates the STOCHRSI, plots the chart and returns the signal.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartIndex` | `number` | Yes | Chart index on which to plot. |
| `name` | `string` | No | Unique name of the indicator. |
| `interval` | `number` | No | Used interval for price data. Default is 0 and the main interval will be used. |

**Returns:** `enum`

Returns a signal based on the GetCrossOverUnderSignal() template.

---

## EasyTRIX

```lua
EasyTRIX(chartIndex, [name], [interval])
```

Automatically defines the input fields, calculates the TRIX, plots the chart and returns the signal.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartIndex` | `number` | Yes | Chart index on which to plot. |
| `name` | `string` | No | Unique name of the indicator. |
| `interval` | `number` | No | Used interval for price data. Default is 0 and the main interval will be used. |

**Returns:** `enum`

Returns a signal based on TRIX, it's MA and selected signal type.

---

## EasyTSI

```lua
EasyTSI(chartIndex, [name], [interval])
```

Automatically defines the input fields, calculates the TSI, plots the chart and returns the signal.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartIndex` | `number` | Yes | Chart index on which to plot. |
| `name` | `string` | No | Unique name of the indicator. |
| `interval` | `number` | No | Used interval for price data. Default is 0 and the main interval will be used. |

**Returns:** `enum`

Returns a signal based on TSI, it's MA and selected signal type.

---

## EasyUDRSI

```lua
EasyUDRSI(chartIndex, [name], [interval])
```

Automatically defines the input fields, calculates the UDRSI, plots the chart and returns the signal.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartIndex` | `number` | Yes | Chart index on which to plot. |
| `name` | `string` | No | Unique name of the indicator. |
| `interval` | `number` | No | Used interval for price data. Default is 0 and the main interval will be used. |

**Returns:** `enum`

Returns a signal based on the GetBuySellLevelSignal() template.

---

## EasyULTOSC

```lua
EasyULTOSC(chartIndex, [name], [interval])
```

Automatically defines the input fields, calculates the ULTOSC, plots the chart and returns the signal.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartIndex` | `number` | Yes | Chart index on which to plot. |
| `name` | `string` | No | Unique name of the indicator. |
| `interval` | `number` | No | Used interval for price data. Default is 0 and the main interval will be used. |

**Returns:** `enum`

Returns a signal based on ULTOSC, it's MA and selected signal type.

---

## EasyWILLR

```lua
EasyWILLR(chartIndex, [name], [interval])
```

Automatically defines the input fields, calculates the WILLR, plots the chart and returns the signal.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartIndex` | `number` | Yes | Chart index on which to plot. |
| `name` | `string` | No | Unique name of the indicator. |
| `interval` | `number` | No | Used interval for price data. Default is 0 and the main interval will be used. |

**Returns:** `enum`

Returns a signal based on the GetBuySellLevelSignal() template.

---

## EasyZLMA

```lua
EasyZLMA(chartIndex, [name], [interval])
```

Automatically defines the input fields, calculates the ZLMA, plots the chart and returns the signal.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `chartIndex` | `number` | Yes | Chart index on which to plot. |
| `name` | `string` | No | Unique name of the indicator. |
| `interval` | `number` | No | Used interval for price data. Default is 0 and the main interval will be used. |

**Returns:** `enum`

Returns a signal based on the GetAboveBelowSignal() template. Signals are produced when faster ZLMA crosses above or below the slower ZLMA.

---


# Technical Analysis

## ABANDS

```lua
ABANDS(high, low, close, period)
```

Acceleration Bands (ABANDS) created by Price Headley.
                            Plots bands around a simple moving average as the midpoint and the upper and lower
                            bands are of equal distance from this midpoint.
                            Look for breakouts outside these bands, while also using the shorter time frames
                            to define likely support and resistance levels at the lower and upper Acceleration Bands. This indicator is usually plotted on the price chart.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `high` | `HaasNumberCollection` | Yes | High prices array. |
| `low` | `HaasNumberCollection` | Yes | Low prices array. |
| `close` | `HaasNumberCollection` | Yes | Close prices array. |
| `period` | `number` | Yes | Period length. |

**Returns:** `dynamic`

Returns an array with 3 results arrays. Upper on index 1, middle on index 2 and lower on index 3.

---

## AD

```lua
AD(high, low, close, volume)
```

Accumulation/Distribution Line (AD) Developed by Marc Chaikin, the Accumulation Distribution
Line is a volume-based indicator designed to measure the cumulative flow of money into and out of a security. Chaikin
originally referred to the indicator as the Cumulative Money Flow Line. As with cumulative indicators, the Accumulation
Distribution Line is a running total of each period's Money Flow Volume. First, a multiplier is calculated based on the
relationship of the close to the high-low range. Second, the Money Flow Multiplier is multiplied by the period's volume
to come up with a Money Flow Volume. A running total of the Money Flow Volume forms the Accumulation Distribution Line. This indicator is usually plotted to its own chart.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `high` | `HaasNumberCollection` | Yes | High prices array. |
| `low` | `HaasNumberCollection` | Yes | Low prices array. |
| `close` | `HaasNumberCollection` | Yes | Close prices array. |
| `volume` | `HaasNumberCollection` | Yes | Market volume array. |

**Returns:** `HaasNumberCollection`

Returns the AD result.

---

## ADOSC

```lua
ADOSC(high, low, close, volume, fastPeriod, slowPeriod)
```

Developed by Marc Chaikin, the Chaikin Oscillator (ADOSC) measures the momentum of the
Accumulation Distribution Line using the MACD formula. (This makes it an indicator of an indicator.)
The Chaikin Oscillator is the difference between the 3-day and 10-day EMAs of the Accumulation Distribution
Line. Like other momentum indicators, this indicator is designed to anticipate directional changes in the
Accumulation Distribution Line by measuring the momentum behind the movements.
A momentum change is the first step to a trend change. This indicator is usually plotted to its own chart.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `high` | `HaasNumberCollection` | Yes | High prices array. |
| `low` | `HaasNumberCollection` | Yes | Low prices array. |
| `close` | `HaasNumberCollection` | Yes | Close prices array. |
| `volume` | `HaasNumberCollection` | Yes | Market volume array. |
| `fastPeriod` | `number` | Yes | Fast period length. |
| `slowPeriod` | `number` | Yes | Slow period length. |

**Returns:** `HaasNumberCollection`

Returns the ADOSC result.

---

## ADX

```lua
ADX(high, low, close, period)
```

The Average Directional Index (ADX) depicts a presence or absence of a trend.
ADX advices on the strength of the dominant forces that move market prices here and now.
In other words, ADX advices on trend tendencies: whether the trend is going to continue
and strengthen or it is about to lose its positions. This indicator is usually plotted to its own chart.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `high` | `HaasNumberCollection` | Yes | High prices array. |
| `low` | `HaasNumberCollection` | Yes | Low prices array. |
| `close` | `HaasNumberCollection` | Yes | Close prices array. |
| `period` | `number` | Yes | Period length. |

**Returns:** `HaasNumberCollection`

Returns the ADX result.

---

## ADXR

```lua
ADXR(high, low, close, period)
```

Average Directional Movement Index Rating (ADXR) is a smoothed version
of ADX indicator and is used as a rating of the Directional Movement while smoothing out ADX values.
If to compare ADXR to ADX, the smoother ADXR doesn't depend much on fast short-term market turns and
reversals, which allows to minimize trading risks when relying on ADXR in the long term. This indicator is usually plotted to its own chart.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `high` | `HaasNumberCollection` | Yes | High prices array. |
| `low` | `HaasNumberCollection` | Yes | Low prices array. |
| `close` | `HaasNumberCollection` | Yes | Close prices array. |
| `period` | `number` | Yes | Period length. |

**Returns:** `HaasNumberCollection`

Returns the ADXR result.

---

## AO

```lua
AO(hl2prices, shortLength, longLength)
```

The Awesome Oscillator (AO) shows the difference in between the 5 SMA and 34 SMA.
If to be precise, 5 SMA of mid-prices is subtracted from 34 SMA of mid-prices which allows to see the market momentum. This indicator is usually plotted to its own chart.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `hl2prices` | `HaasNumberCollection` | Yes | Middle prices array ([High + Low] / 2). |
| `shortLength` | `number` | Yes | Shorter period length. |
| `longLength` | `number` | Yes | Longer period length. |

**Returns:** `HaasNumberCollection`

Returns the AO result.

---

## APO

```lua
APO(prices, fastPeriod, slowPeriod, [maType])
```

The Absolute Price Oscillator (APO) displays the difference between two
exponential moving averages of an asset's price and is expressed as an absolute value. This indicator is usually plotted to its own chart.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `prices` | `HaasNumberCollection` | Yes | Source data. |
| `fastPeriod` | `number` | Yes | Faster period length. |
| `slowPeriod` | `number` | Yes | Slower period length. |
| `maType` | `enum` | No | MA type used in the calculation. |

**Returns:** `HaasNumberCollection`

Returns the APO result.

---

## AROON

```lua
AROON(high, low, period)
```

Aroon indicator is a young one, developed by Tushar Chande in 1995.
Aroon was created to measure the strength of a trend and potentials for its continuation as well
as the quality and type of the trend: up-trend, down-trend or sideways moving market. This indicator is usually plotted to its own chart.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `high` | `HaasNumberCollection` | Yes | High prices array. |
| `low` | `HaasNumberCollection` | Yes | Low prices array. |
| `period` | `number` | Yes | Period length. |

**Returns:** `dynamic`

Returns an array with 2 results arrays. Aroon down on index 1 & Aroon up on index 2.

---

## AROONOSC

```lua
AROONOSC(high, low, period)
```

Aroon oscillator is based on Aroon Indicator. Aroon Oscillator is
a trend-following indicator that illustrates the strength of a current trend and its potentials to last. This indicator is usually plotted to its own chart.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `high` | `HaasNumberCollection` | Yes | High prices array. |
| `low` | `HaasNumberCollection` | Yes | Low prices array. |
| `period` | `number` | Yes | Period length. |

**Returns:** `HaasNumberCollection`

Returns the AROONOSC result.

---

## ATR

```lua
ATR(high, low, close, period)
```

The Average True Range (ATR), developed by Wilder, gives traders a feel of what the historical volatility
was in order to prepare for trading in the actual market.
Currency pairs that get lower ATR readings suggest lower market volatility, while currency pairs with higher
ATR indicator readings require appropriate trading adjustments according to higher volatility. 
Wilder used the Moving average to smooth out the ATR indicator readings, so that ATR looks the way we know it. This indicator is usually plotted to its own chart.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `high` | `HaasNumberCollection` | Yes | High prices array. |
| `low` | `HaasNumberCollection` | Yes | Low prices array. |
| `close` | `HaasNumberCollection` | Yes | Close prices array. |
| `period` | `number` | Yes | Period length. |

**Returns:** `HaasNumberCollection`

Returns the ATR result.

---

## AVGDEV

```lua
AVGDEV(close, period)
```

The Average Deviation (AVGDEV) is simply an averaged version of the Standard Deviation (STDDEV). This indicator is usually plotted to its own chart.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `close` | `HaasNumberCollection` | Yes | Close prices array. |
| `period` | `number` | Yes | Period length. |

**Returns:** `HaasNumberCollection`

Returns the AVGDEV result.

---

## AVGPRICE

```lua
AVGPRICE(open, high, low, close)
```

The Average Price (AVGPRICE) is the price between open, high, low and close (aka OHLC/4). This indicator is usually plotted on the price chart.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `open` | `HaasNumberCollection` | Yes | Open prices array. |
| `high` | `HaasNumberCollection` | Yes | High prices array. |
| `low` | `HaasNumberCollection` | Yes | Low prices array. |
| `close` | `HaasNumberCollection` | Yes | Close prices array. |

**Returns:** `HaasNumberCollection`

Returns the AVGPRICE result.

---

## BBANDS

```lua
BBANDS(prices, period, devUp, devDn, [maType])
```

The Bollinger Band (BBANDS) study created
                            by John Bollinger plots upper and lower envelope bands around the
                            price of the instrument. The width of the bands is based on the
                            standard deviation of the closing prices from a moving average of
                            price. This indicator is usually plotted on the price chart.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `prices` | `HaasNumberCollection` | Yes | Source data. |
| `period` | `number` | Yes | Period length. |
| `devUp` | `number` | Yes | Standard deviation above the middle bands. |
| `devDn` | `number` | Yes | Standard deviation below the middle bands. |
| `maType` | `enum` | No | MA type used in the calculation. |

**Returns:** `dynamic`

Returns an array with 3 results arrays. Upper on index 1, middle on index 2 and lower on index 3.

---

## BETA

```lua
BETA(prices1, prices2, period)
```

The Beta (BETA) value can measure he volatility of a coin in relation to that of the market.
For stock investors the beta value[1] of a portfolio is an important measure of risk, a value above 1 means the
investment is more volatile than the market, a value lower than 1 is less volatile.
It can be helpful to do a beta analysis for your portfolio to see how returns match up against the beta and
spot real winners (low beta, high returns). Since most altcoins are still sold and bought using Bitcoin (BTC),
its price could be used as the [Market's source data] (2nd input parameter), which results BTC a beta of 1. This indicator is usually plotted to its own chart.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `prices1` | `HaasNumberCollection` | Yes | Asset's source data. |
| `prices2` | `HaasNumberCollection` | Yes | Market's source data. |
| `period` | `number` | Yes | Lookback period length. |

**Returns:** `HaasNumberCollection`

Returns the BETA result.

---

## BOP

```lua
BOP(open, high, low, close)
```

The Balance of Power (BOP) indicator measures the market
strength of buyers against sellers by assessing the ability of each side to drive prices to an extreme level.
The resulting values can be smoothed by a moving average. This indicator is usually plotted to its own chart.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `open` | `HaasNumberCollection` | Yes | Open prices array. |
| `high` | `HaasNumberCollection` | Yes | High prices array. |
| `low` | `HaasNumberCollection` | Yes | Low prices array. |
| `close` | `HaasNumberCollection` | Yes | Close prices array. |

**Returns:** `HaasNumberCollection`

Returns the BOP result.

---

## CCI

```lua
CCI(high, low, close, period)
```

The Commodity Channel Index (CCI) compares the current price to an
average price over a period of time. The indicator fluctuates above or below zero,
moving into positive or negative territory. CCI is relatively high when prices are
far above their average, but is relatively low when prices are far below their average.
In this manner, CCI can be used to identify overbought and oversold levels, or to confirm trends. This indicator is usually plotted to its own chart.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `high` | `HaasNumberCollection` | Yes | High prices array. |
| `low` | `HaasNumberCollection` | Yes | Low prices array. |
| `close` | `HaasNumberCollection` | Yes | Close prices array. |
| `period` | `number` | Yes | Period length. |

**Returns:** `HaasNumberCollection`

Returns the CCI result.

---

## CDL

```lua
CDL(open, high, low, close, type, [penetration])
```

Candlestick pattern finder.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `open` | `HaasNumberCollection` | Yes | Open prices array. |
| `high` | `HaasNumberCollection` | Yes | High prices array. |
| `low` | `HaasNumberCollection` | Yes | Low prices array. |
| `close` | `HaasNumberCollection` | Yes | Close prices array. |
| `type` | `enum` | Yes | The Candle Pattern (CDL) type. |
| `penetration` | `number` | No | Penetration threshold. |

**Returns:** `HaasNumberCollection`

Returns a value. 0 is not found. Above 0 is long pattern found. Below 0 is short pattern found.

---

## ChandelierExitLong

```lua
ChandelierExitLong(high, close, low, [depth], [multiplier])
```

Calculates the chandelier exit long price.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `high` | `HaasNumberCollection` | Yes | High prices array. |
| `close` | `HaasNumberCollection` | Yes | Close prices array. |
| `low` | `HaasNumberCollection` | Yes | Low prices array. |
| `depth` | `number` | No | Depth length. Default is 20. |
| `multiplier` | `number` | No | ATR multiplier. Default is 3. |

**Returns:** `number`

Returns the target price.

---

## ChandelierExitShort

```lua
ChandelierExitShort(high, close, low, [depth], [multiplier])
```

Calculates the chandelier exit short price.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `high` | `HaasNumberCollection` | Yes | High prices array. |
| `close` | `HaasNumberCollection` | Yes | Close prices array. |
| `low` | `HaasNumberCollection` | Yes | Low prices array. |
| `depth` | `number` | No | Depth length. Default is 20. |
| `multiplier` | `number` | No | ATR multiplier. Default is 3. |

**Returns:** `number`

Returns the target price.

---

## CHOP

```lua
CHOP(high, low, close, period)
```

The Choppiness Index, designed by E.W. Dreiss, is an indicator which is directionless. This means it doesn't show the market direction,
                            rather reflects whether the market is choppy (trades sideways) or smooth (trending). The general way of interpreting
                            this index is that higher values mean the market is choppy, while lower values equal directional movement.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `high` | `HaasNumberCollection` | Yes | High prices array. |
| `low` | `HaasNumberCollection` | Yes | Low prices array. |
| `close` | `HaasNumberCollection` | Yes | Close prices array. |
| `period` | `number` | Yes | Period length. |

**Returns:** `HaasNumberCollection`

Returns the CHOP result.

---

## CMO

```lua
CMO(prices, period)
```

The Chande Momentum Oscillator (CMO) indicates overbought
conditions when it reaches the 50 level and oversold conditions when it reaches −50.
You can also look for signals based on the CMO crossing above and below a signal line
composed of a 9-period moving average of the 20 period CMO. This indicator measures the trend strength. This indicator is usually plotted to its own chart.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `prices` | `HaasNumberCollection` | Yes | Source data. |
| `period` | `number` | Yes | Period length. |

**Returns:** `HaasNumberCollection`

Returns the CMO result.

---

## COPPOCK

```lua
COPPOCK(prices, roc1, roc2, period)
```

The Coppock Curve (COPPOCK) is a smoothed momentum oscillator developed by Edwin “Sedge” Coppock.
It can help you isolate buying opportunities and exit points. Even though it was originally designed for long-term analysis,
it can be used on intraday, daily or weekly charts and the settings can be adjusted to suit one's style.
The main signals are generated with crosses above and below the zero line. This indicator is usually plotted to its own chart.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `prices` | `HaasNumberCollection` | Yes | Source data. |
| `roc1` | `number` | Yes | Period length of first ROC. |
| `roc2` | `number` | Yes | Period length of second ROC. |
| `period` | `number` | Yes | Period length. |

**Returns:** `HaasNumberCollection`

Returns the COPPOCK result.

---

## CORREL

```lua
CORREL(source1, source2, period)
```

The Correlation (CORREL) is a statistical measure that reflects the correlation between two assets. 
In other words, this statistic tells us how closely one security is related to the other. 
Coefficient is above zero when both securities move in the same direction (up or down) and below zero when the two securities move in opposite directions. 
This indicator can also help traders diversify by identifying assets with a low or negative correlation to the market. This indicator is usually plotted to its own chart.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `source1` | `HaasNumberCollection` | Yes | First source data. |
| `source2` | `HaasNumberCollection` | Yes | Second source data. |
| `period` | `number` | Yes | Period length. |

**Returns:** `HaasNumberCollection`

Returns the CORREL result.

---

## CRSI

```lua
CRSI(prices, rsiPeriod, udPeriod, length)
```

The Connor's RSI (CRSI) is a momentum oscillator created by Larry Connors.
Three components combine to form this momentum oscillator; The RSI, UpDown Length, and Rate-of-Change.
Connors RSI outputs a value between 0 and 100, which is then used to identify short-term overbought
and oversold conditions, or to confirm trends. This indicator is usually plotted to its own chart.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `prices` | `HaasNumberCollection` | Yes | Source data. |
| `rsiPeriod` | `number` | Yes | RSI period length. |
| `udPeriod` | `number` | Yes | UD period length. |
| `length` | `number` | Yes | Period length. |

**Returns:** `HaasNumberCollection`

Returns the CRSI result.

---

## DEMA

```lua
DEMA(prices, period)
```

The Double Exponential Moving Average (DEMA) was developed by Patrick Mulloy.
It is used for smoothing price series and attempts to offer a smoothed average with less
lag than a straight exponential moving average (EMA). This indicator is usually plotted directly on a price chart.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `prices` | `HaasNumberCollection` | Yes | Source data. |
| `period` | `number` | Yes | Period length. |

**Returns:** `HaasNumberCollection`

Returns the DEMA result.

---

## DONCHIAN

```lua
DONCHIAN(high, low, period)
```

The Donchian Channel (DONCHIAN), developed by Richard Donchian.
Donchian Channel is formed by taking the highest high and the lowest low of the last n periods.
The area between the high and the low is the channel for the period chosen. This indicator is usually plotted on the price chart.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `high` | `HaasNumberCollection` | Yes | High prices array. |
| `low` | `HaasNumberCollection` | Yes | Low prices array. |
| `period` | `number` | Yes | Period length. |

**Returns:** `dynamic`

Returns an array with 3 results arrays. Upper on index 1, middle on index 2 and lower on index 3.

---

## DPO

```lua
DPO(prices, shortLength, longLength)
```

The Detrended Price Oscillator (DPO) is an indicator
in technical analysis that attempts to eliminate the long-term trends in prices
by using a displaced moving average so it does not react to the most current price action.
This allows the indicator to show intermediate overbought and oversold levels effectively.
This indicator is usually plotted on its own chart.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `prices` | `HaasNumberCollection` | Yes | Source data. |
| `shortLength` | `number` | Yes | Shorter period length. |
| `longLength` | `number` | Yes | Longer period length. |

**Returns:** `HaasNumberCollection`

Returns the DPO result.

---

## DX

```lua
DX(high, low, close, period)
```

The Directional Movement Index (DX) is indicator in technical
analysis which is based on two other Welles Wilder's indicators: negative directional
indicator (-DI) and positive directional indicator (+DI). It was designed to evaluate
the strength of a trend and to define a period of sideways trading. This indicator is usually
plotted on its own chart.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `high` | `HaasNumberCollection` | Yes | High prices array. |
| `low` | `HaasNumberCollection` | Yes | Low prices array. |
| `close` | `HaasNumberCollection` | Yes | Close prices array. |
| `period` | `number` | Yes | Period length. |

**Returns:** `HaasNumberCollection`

Returns the DX result.

---

## EMA

```lua
EMA(prices, period)
```

The Exponential Moving Average (EMA) is a weighted moving average
(WMA) that gives more weighting, or importance, to recent price data than the simple moving average (SMA) does.
This indicator is usually plotted on the price chart.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `prices` | `HaasNumberCollection` | Yes | Source data. |
| `period` | `number` | Yes | Period length. |

**Returns:** `HaasNumberCollection`

Returns the EMA result.

---

## FastRSI

```lua
FastRSI(prices, period, signal)
```

The FastRSI is a custom implementation from Haasonline. It uses a custom RSI and EMA calculation.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `prices` | `HaasNumberCollection` | Yes | Source data. |
| `period` | `number` | Yes | Period length. |
| `signal` | `number` | Yes | Signal length. |

**Returns:** `dynamic`

Returns the FastRSI result.

---

## HT_DCPERIOD

```lua
HT_DCPERIOD(prices)
```

Hilbert Transform - The Dominant Cycle Period (HT_DCPERIOD).
The Hilbert Transform is a technique used to generate inphase and quadrature components
of a de-trended real-valued "analytic-like" signal (such as a Price Series) in order
to analyze variations of the instantaneous phase and amplitude. HTPeriod (or MESA Instantaneous Period)
returns the period of the Dominant Cycle of the analytic signal as generated by the Hilbert Transform.
The Dominant Cycle can be thought of as being the "most likely" period (in the range of 10 to 40)
of a sine function of the Price Series.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `prices` | `HaasNumberCollection` | Yes | Source data. |

**Returns:** `HaasNumberCollection`

Returns the HT_DCPERIOD result.

---

## HT_DCPHASE

```lua
HT_DCPHASE(prices)
```

Hilbert Transform - The Dominant Cycle Phase (HT_DCPHASE)
The Hilbert Transform is a technique used to generate inphase and quadrature components
of a de-trended real-valued "analytic-like" signal (such as a Price Series) in order
to analyze variations of the instantaneous phase and amplitude. HTDCPhase returns the
Hilbert Transform Phase of the Dominant Cycle. The Dominant Cycle Phase lies in the
range of 0 to 360 degrees.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `prices` | `HaasNumberCollection` | Yes | Source data. |

**Returns:** `HaasNumberCollection`

Returns the HT_DCPHASE result.

---

## HT_PHASOR

```lua
HT_PHASOR(prices)
```

Hilbert Transform - The Phasor Components (HT_PHASOR)
The Hilbert Transform is a technique used to generate inphase and quadrature components
of a de-trended real-valued "analytic-like" signal (such as a Price Series) in order
to analyze variations of the instantaneous phase and amplitude.
In-Phase is the Hilbert Transform generated In-Phase component of the input Price Series.
Quadrature is the Hilbert Transform generated Quadrature component of the input Price Series.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `prices` | `HaasNumberCollection` | Yes | Source data. |

**Returns:** `dynamic`

Returns the HT_PHASOR result.

---

## HT_SINE

```lua
HT_SINE(prices)
```

Hilbert Transform - The SineWave (HT_SINE)
The Hilbert Transform is a technique used to generate inphase and quadrature components
of a de-trended real-valued "analytic-like" signal (such as a Price Series) in order
to analyze variations of the instantaneous phase and amplitude. HTDCPhase returns the
Hilbert Transform Phase of the Dominant Cycle. The Dominant Cycle Phase lies in the
range of 0 to 360 degrees.
The Hilbert Transform Sine is just the sine of the DC Phase. 
The Hilbert Transform Lead Sine is just the sine of the DC Phase advanced by 45 degrees.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `prices` | `HaasNumberCollection` | Yes | Source data. |

**Returns:** `dynamic`

Returns the HT_SINE result.

---

## HT_TRENDLINE

```lua
HT_TRENDLINE(prices)
```

Hilbert Transform - The Trend Line (HT_TRENDLINE)
The Hilbert Transform is a technique used to generate inphase and quadrature components
of a de-trended real-valued "analytic-like" signal (such as a Price Series) in order
to analyze variations of the instantaneous phase and amplitude. HTTrendline (or MESA Instantaneous Trendline)
returns the Price Series value after the Dominant Cycle of the analytic signal as generated by the Hilbert
Transform has been removed. The Dominant Cycle can be thought of as being the "most likely" period
(in the range of 10 to 40) of a sine function of the Price Series.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `prices` | `HaasNumberCollection` | Yes | Source data. |

**Returns:** `HaasNumberCollection`

Returns the HT_TRENDLINE result.

---

## HT_TRENDMODE

```lua
HT_TRENDMODE(prices)
```

Hilbert Transform - The Trend Mode (HT_TRENDMODE).

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `prices` | `HaasNumberCollection` | Yes | Source data. |

**Returns:** `HaasNumberCollection`

Returns the HT_TRENDMODE result.

---

## ICHIMOKU

```lua
ICHIMOKU(prices, tenkansen, kijunsen, senkouspanb)
```

The Ichimoku Clouds (ICHIMOKU) also known as Ichimoku Kinko Hyo, developed by journalist Goichi Hosoda,
is a versatile indicator that defines support and resistance, identifies trend direction, gauges momentum and provides trading signals.
Ichimoku Kinko Hyo translates into “one look equilibrium chart”. With one look, chartists
can identify the trend and look for potential signals within that trend. Even though the
Ichimoku Cloud may seem complicated when viewed on the price chart, it's actually a rather
straightforward indicator; the concepts are easy to understand and the signals are well-defined.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `prices` | `HaasNumberCollection` | Yes | Source data. |
| `tenkansen` | `number` | Yes | Period length for Tenkan-sen. |
| `kijunsen` | `number` | Yes | Period length for Kijun-sen. |
| `senkouspanb` | `number` | Yes | Period length for Senkou Span B. |

**Returns:** `dynamic`

Returns the ICHIMOKU result.

---

## IMI

```lua
IMI(open, close, period)
```

The Intraday Momentum Index (IMI), developed by Tushar Chande,
combines aspects of candlestick analysis with the relative strength index (RSI). The intraday
indicator was designed to provide investors with a way to find optimal days to buy and sell.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `open` | `HaasNumberCollection` | Yes | Open prices array. |
| `close` | `HaasNumberCollection` | Yes | Close prices array. |
| `period` | `number` | Yes | Period length. |

**Returns:** `HaasNumberCollection`

Returns the IMI result.

---

## KAMA

```lua
KAMA(prices, period, [fastest], [slowest])
```

The Kaufman's Adaptive Moving Average (KAMA), developed by Perry Kaufman,
is an intelligent moving average. The powerful trend-following indicator is based on the
Exponential Moving Average (EMA) and is responsive to both trend and volatility.
Like all moving averages, the KAMA can be used to visualize the trend. This indicator is usually plotted on the price chart.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `prices` | `HaasNumberCollection` | Yes | Source data. |
| `period` | `number` | Yes | Period length. |
| `fastest` | `number` | No | Fastest smoothing period length. |
| `slowest` | `number` | No | Slowest smoothing period length. |

**Returns:** `HaasNumberCollection`

Returns the KAMA result.

---

## KAMA2

```lua
KAMA2(prices, period, [fastest], [slowest])
```

The Kaufman's Adaptive Moving Average (KAMA2), developed by Perry Kaufman,
is an intelligent moving average. The powerful trend-following indicator is based on the
Exponential Moving Average (EMA) and is responsive to both trend and volatility.
Like all moving averages, the KAMA can be used to visualize the trend. This indicator is usually plotted on the price chart.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `prices` | `HaasNumberCollection` | Yes | Source data. |
| `period` | `number` | Yes | Period length. |
| `fastest` | `number` | No | Fastest smoothing period length. |
| `slowest` | `number` | No | Slowest smoothing period length. |

**Returns:** `HaasNumberCollection`

Returns the KAMA2 result.

---

## KELTNER

```lua
KELTNER(high, low, close, emaPeriod, [atrPeriod], [multiplier])
```

The Keltner Channel (KELTNER), originally developed by Chester Keltner, extended by Linda Bradford Raschke,
uses a volatility-based envelopes set above and below an exponential moving average.
This indicator is similar to Bollinger Bands, which use the standard deviation (STDDEV)
to set the bands. Instead of using the standard deviation, Keltner Channels use
the Average True Range (ATR) to set channel width. The channels are typically
set 2x ATR values above and below the EMA. The exponential
moving average dictates direction and the Average True Range sets channel width.
Keltner Channels are a trend following indicator used to identify reversals with
channel breakouts and channel direction. Channels can also be used to identify
overbought and oversold levels when the market is ranging. This indicator is usually plotted on the price chart.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `high` | `HaasNumberCollection` | Yes | High prices array. |
| `low` | `HaasNumberCollection` | Yes | Low prices array. |
| `close` | `HaasNumberCollection` | Yes | Close prices array. |
| `emaPeriod` | `number` | Yes | EMA period length. |
| `atrPeriod` | `number` | No | ATR period length. |
| `multiplier` | `number` | No | ATR multiplier. |

**Returns:** `dynamic`

Returns the KELTNER result.

---

## KRI

```lua
KRI(prices, period)
```

Kairi method (KRI) is similar to Momentum according to its application mode.
The oscillator fluctuates around 0 but the fluctuation range is wider. KRI can be used for any time frame.
It is one of the simplest oscillators. When creating the indicator, deviation of a price from its simple
moving average is calculated and the result is shown in percentage of the average.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `prices` | `HaasNumberCollection` | Yes | Source data. |
| `period` | `number` | Yes | Period length. |

**Returns:** `HaasNumberCollection`

Returns the KRI result.

---

## KST

```lua
KST(prices, rocPeriod1, rocSmaPeriod1, rocPeriod2, rocSmaPeriod2, rocPeriod3, rocSmaPeriod3, rocPeriod4, rocSmaPeriod4, signalPeriod)
```

Know Sure Thing (KST), developed by Martin Pring, is a momentum oscillator based on the smoothed
Rate-of-Change (ROC) for four different periods. In short, KST measures price momentum for four different price cycles, combining
them into a single momentum oscillator. Like any other unbound momentum oscillator, chartists can use KST to look for divergences, signal
line crossovers, and centerline crossovers. Pring frequently applied trend lines to KST. Although trend line signals do not occur
often, Pring notes that such breaks reinforce signal line crossovers. This indicator is usually plotted on its own chart.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `prices` | `HaasNumberCollection` | Yes | Source data. |
| `rocPeriod1` | `number` | Yes | 1st ROC period length. |
| `rocSmaPeriod1` | `number` | Yes | 1st SMA-of-ROC period length. |
| `rocPeriod2` | `number` | Yes | 2nd ROC period length. |
| `rocSmaPeriod2` | `number` | Yes | 2nd SMA-of-ROC period length. |
| `rocPeriod3` | `number` | Yes | 3rd ROC period length. |
| `rocSmaPeriod3` | `number` | Yes | 3rd SMA-of-ROC period length. |
| `rocPeriod4` | `number` | Yes | 4th ROC period length. |
| `rocSmaPeriod4` | `number` | Yes | 4th SMA-of-ROC period length. |
| `signalPeriod` | `number` | Yes | Signal period length. |

**Returns:** `dynamic`

Returns the KST result.

---

## LINEARREG

```lua
LINEARREG(prices, period, [type])
```

The Linear Regression (LINEARREG)
is a trend following indicator that plots a dynamic version of the linear
regression indicator. The concept is to track the trend not using basic
averages or weighted averages – as in the case of moving averages – but
rather by taking the "best fit" line to match the data.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `prices` | `HaasNumberCollection` | Yes | Source data. |
| `period` | `number` | Yes | Period length. |
| `type` | `enum` | No | The type of the Linear Regression. |

**Returns:** `HaasNumberCollection`

Returns the LINEARREG result.

---

## MA

```lua
MA(prices, period, maType)
```

The Moving Averages (MA) combines multiple Moving Averages
in one command. Works well together with the InputMaTypes() command. Supported types are:
'sma', 'ema', 'dema', 'tema', 'trima', 't3', 'mama', 'kama' and 'wma'.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `prices` | `HaasNumberCollection` | Yes | Source data. |
| `period` | `number` | Yes | Period length. |
| `maType` | `enum` | Yes | The type of Moving Average. |

**Returns:** `HaasNumberCollection`

Returns the MA result.

---

## MACD

```lua
MACD(prices, fastPeriod, slowPeriod, signalPeriod)
```

The Moving Average Convergence Divergence (MACD), developed by ,
is a trend-following momentum indicator that shows the relationship between two moving averages of an asset's price.
The MACD is calculated by subtracting the longer-period Exponential Moving Average (EMA) from the shorter-period EMA.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `prices` | `HaasNumberCollection` | Yes | Source data. |
| `fastPeriod` | `number` | Yes | Fast period length. |
| `slowPeriod` | `number` | Yes | Slow period length. |
| `signalPeriod` | `number` | Yes | Signal period length. |

**Returns:** `dynamic`

Returns the MACD result.

---

## MACDEXT

```lua
MACDEXT(prices, fastPeriod, fastType, slowPeriod, slowType, signalPeriod, signalType)
```

The Moving Average Convergence Divergence [Extended] (MACDEXT) is
the same as MACD, but provides more parameters to fiddle with.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `prices` | `HaasNumberCollection` | Yes | Source data. |
| `fastPeriod` | `number` | Yes | Fast period length. |
| `fastType` | `enum` | Yes | Fast MA type. |
| `slowPeriod` | `number` | Yes | Slow period length. |
| `slowType` | `enum` | Yes | Slow MA type. |
| `signalPeriod` | `number` | Yes | Signal period length. |
| `signalType` | `enum` | Yes | Signal MA type. |

**Returns:** `dynamic`

Returns the MACDEXT result.

---

## MACDFIX

```lua
MACDFIX(prices, signalPeriod)
```

The Moving Average Convergence Divergence [Fixed] (MACDFIX) is
the same as MACD, but uses fixed periods for the fast (12) and slow (26) EMAs.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `prices` | `HaasNumberCollection` | Yes | Source data. |
| `signalPeriod` | `number` | Yes | Signal period length. |

**Returns:** `dynamic`

Returns the MACDFIX result.

---

## MAMA

```lua
MAMA(prices, fastlimit, slowlimit)
```

The Mesa Adaptive Moving Average (MAMA) is a trend-following
indicator which adapts to price movement "based on the rate change of phase as measured by the Hilbert Transform Discriminator".
This method of adaptation features a fast and a slow moving average so that the composite moving average swiftly responds to price
changes and holds the average value until the next bar’s close. 
The Averages act as support and resistance areas and the price will tend to react to them.
This makes pullbacks to the MAMA and FAMA suitable with-trend entry areas. This indicator is usually plotted on the price chart.
NOTE: The FastLimit and SlowLimit parameters should be between 0.01 and 0.99.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `prices` | `HaasNumberCollection` | Yes | Source data. |
| `fastlimit` | `number` | Yes | Limit for fastest speed (bigger = faster). |
| `slowlimit` | `number` | Yes | Limit for slowest speed (smaller = slower). |

**Returns:** `dynamic`

Returns the MAMA result.

---

## MAVP

```lua
MAVP(prices, inPeriods, minPeriod, maxPeriod, [maType])
```

The Moving Average Variable Period (MAVP) is a moving average
that adjusts itself based on the period settings given to it. The [inPeriods] array should be the
same length as [prices] array. The Nth period will be used to calculate the Nth data point.
It is essentially the same as changing the period yourself using the MA() command, but using this
command instead, you can use for example the absolute (ABS) CMO output values as the periods:
local periods = Abs(CMO(ClosePrices(), 20)).

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `prices` | `HaasNumberCollection` | Yes | Source data. |
| `inPeriods` | `HaasNumberCollection` | Yes | Array of period lengths (must be equal length to source data!). |
| `minPeriod` | `number` | Yes | Minimum allowed period (fastest). |
| `maxPeriod` | `number` | Yes | Maximum allowed period (slowest). |
| `maType` | `enum` | No | Type of the Moving Average. |

**Returns:** `HaasNumberCollection`

Returns the MAVP result.

---

## MAXINDEX

```lua
MAXINDEX(prices, period)
```

Max Index (MAXINDEX) will return the index of the
highest high in source data within the given period. You can see this index as the "distance"
to the highest high. To get the highest high value, use this index with the input
array or use GetHigh/GetHighs command instead.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `prices` | `HaasNumberCollection` | Yes | Source data. |
| `period` | `number` | Yes | Period length. |

**Returns:** `number`

Returns the MAXINDEX result.

---

## MEDPRICE

```lua
MEDPRICE(high, low)
```

Median Price (MEDPRICE) indicator is simply the midpoint of market's prices.
The Typical Price and Weighted Close are similar indicators. The Median Price indicator
provides a simple, single-line chart of the market's"average price."
This average price is useful when you want a simpler view of prices.
This indicator is usually plotted on the price chart.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `high` | `HaasNumberCollection` | Yes | High prices array. |
| `low` | `HaasNumberCollection` | Yes | Low prices array. |

**Returns:** `HaasNumberCollection`

Returns the MEDPRICE result.

---

## MFI

```lua
MFI(high, low, close, volume, period)
```

The Money Flow Index (MFI), developed by Gene Quong and Avrum Soudack,
is an oscillator that uses both price and volume to measure buying and selling pressure.
MFI is also known as volume-weighted RSI. MFI starts with the typical price for each period.
Money flow is positive when the typical price rises (buying pressure) and negative when the
typical price declines (selling pressure). A ratio of positive and negative money flow is
then plugged into an RSI formula to create an oscillator that moves between zero and one hundred.
As a momentum oscillator tied to volume, MFI is best suited to identify reversals and price
extremes with a variety of signals.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `high` | `HaasNumberCollection` | Yes | High prices array. |
| `low` | `HaasNumberCollection` | Yes | Low prices array. |
| `close` | `HaasNumberCollection` | Yes | Close prices array. |
| `volume` | `HaasNumberCollection` | Yes | Market volume array. |
| `period` | `number` | Yes | Period length. |

**Returns:** `HaasNumberCollection`

Returns the MFI result.

---

## MIDPOINT

```lua
MIDPOINT(prices, period)
```

Midpoint price (MIDPOINT) is similar to the
 Midprice (MIDPRICE), except the highest and lowest values are returned from
 the same input field; the source data. Midpoint indicator calculates the highest
 close and lowest close within the look back period and averages
 the two values.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `prices` | `HaasNumberCollection` | Yes | Source data. |
| `period` | `number` | Yes | Period length. |

**Returns:** `HaasNumberCollection`

Returns the MIDPOINT result.

---

## MIDPRICE

```lua
MIDPRICE(high, low, period)
```

MidPrice (MIDPRICE) returns the midpoint value from
 two different input fields. The Midprice indicator calculates the
 highest high and lowest low within the look back period and averages
 the two values to return the Midprice.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `high` | `HaasNumberCollection` | Yes | High prices array. |
| `low` | `HaasNumberCollection` | Yes | Low prices array. |
| `period` | `number` | Yes | Period length. |

**Returns:** `HaasNumberCollection`

Returns the MIDPRICE result.

---

## MININDEX

```lua
MININDEX(prices, period)
```

Min Index (MININDEX) will return the index of the
lowest low in source data within the given period. You can see this index as the "distance"
to the lowest low. To get the lowest low value, use this index with the input
array or use GetLow/GetLows command instead.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `prices` | `HaasNumberCollection` | Yes | Source data. |
| `period` | `number` | Yes | Period length. |

**Returns:** `number`

Returns the MININDEX result.

---

## MINUSDI

```lua
MINUSDI(high, low, close, period)
```

The Negative Directional indicator (MINUSDI) measures the presence of a
downtrend and is part of the Average Directional Index (ADX). If -DI is sloping upward, it's a sign
that the price downtrend is getting stronger. This indicator is nearly always plotted along with
the Positive Directional indicator (PLUSDI).

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `high` | `HaasNumberCollection` | Yes | High prices array. |
| `low` | `HaasNumberCollection` | Yes | Low prices array. |
| `close` | `HaasNumberCollection` | Yes | Close prices array. |
| `period` | `number` | Yes | Period length. |

**Returns:** `HaasNumberCollection`

Returns the MINUSDI result.

---

## MINUSDM

```lua
MINUSDM(high, low, period)
```

The Negative Directional Movement (MINUSDM) indicator is the basis of
the Negative Directional indicator (MINUSDI).

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `high` | `HaasNumberCollection` | Yes | High prices array. |
| `low` | `HaasNumberCollection` | Yes | Low prices array. |
| `period` | `number` | Yes | Period length. |

**Returns:** `HaasNumberCollection`

Returns the MINUSDM result.

---

## MOM

```lua
MOM(prices, period)
```

The Momentum Indicator (MOM) is a leading indicator measuring a
asset's rate-of-change. It compares the current price with the previous price from a number
of periods ago. The ongoing plot forms an oscillator that moves above and below 0.
This indicator is usually plotted on its own chart.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `prices` | `HaasNumberCollection` | Yes | Source data. |
| `period` | `number` | Yes | Period length. |

**Returns:** `HaasNumberCollection`

Returns the MOM result.

---

## NATR

```lua
NATR(high, low, close, period)
```

The Normalized Average True Range (NATR) is the normalized version
of the Average True Range (ATR) indicator.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `high` | `HaasNumberCollection` | Yes | High prices array. |
| `low` | `HaasNumberCollection` | Yes | Low prices array. |
| `close` | `HaasNumberCollection` | Yes | Close prices array. |
| `period` | `number` | Yes | Period length. |

**Returns:** `HaasNumberCollection`

Returns the NATR result.

---

## OBV

```lua
OBV(prices, volumes)
```

The On-Balance Volume (OBV), developed by Joseph Granville,
is a momentum indicator that uses volume flow to predict changes in asset price.
He believed that when volume increases sharply without a significant change in the asset's price,
the price will eventually jump upward or fall downward.
This indicator is usually plotted on its own chart.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `prices` | `HaasNumberCollection` | Yes | Source data. |
| `volumes` | `HaasNumberCollection` | Yes | Market volume array. |

**Returns:** `HaasNumberCollection`

Returns the OBV result.

---

## PLUSDI

```lua
PLUSDI(high, low, close, period)
```

The Positive Directional indicator (PLUSDI) measures the presence of an
uptrend and is part of the Average Directional Index (ADX). If +DI is sloping upward, it's a sign
that the price uptrend is getting stronger. This indicator is nearly always plotted along with
the Negative Directional indicator (MINUSDI).

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `high` | `HaasNumberCollection` | Yes | High prices array. |
| `low` | `HaasNumberCollection` | Yes | Low prices array. |
| `close` | `HaasNumberCollection` | Yes | Close prices array. |
| `period` | `number` | Yes | Period length. |

**Returns:** `HaasNumberCollection`

Returns the PLUSDI result.

---

## PLUSDM

```lua
PLUSDM(high, low, period)
```

The Positive Directional Movement (PLUSDM) indicator is the basis of
the Negative Directional indicator (PLUSDI).

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `high` | `HaasNumberCollection` | Yes | High prices array. |
| `low` | `HaasNumberCollection` | Yes | Low prices array. |
| `period` | `number` | Yes | Period length. |

**Returns:** `HaasNumberCollection`

Returns the PLUSDM result.

---

## PPO

```lua
PPO(prices, fastPeriod, slowPeriod, maType)
```

The Percentage Price Oscillator (PPO) is a momentum oscillator
that measures the difference between two moving averages as a percentage of the larger
moving average. As with its cousin, MACD, the Percentage Price Oscillator is shown with
a signal line, a histogram and a centerline.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `prices` | `HaasNumberCollection` | Yes | Source data. |
| `fastPeriod` | `number` | Yes | Fast period length. |
| `slowPeriod` | `number` | Yes | Slow period length. |
| `maType` | `enum` | Yes | The type of Moving Average used. |

**Returns:** `HaasNumberCollection`

Returns the PPO result.

---

## ROC

```lua
ROC(prices, period)
```

The Rate-of-Change (ROC) is often used when speaking about momentum,
and it can generally be expressed as a ratio between a change in one variable relative to a
corresponding change in another; graphically, the rate of change is represented by the slope
of a line. The ROC is often illustrated by the Greek letter delta.
This indicator is usually plotted on its own chart.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `prices` | `HaasNumberCollection` | Yes | Source data. |
| `period` | `number` | Yes | Period length. |

**Returns:** `HaasNumberCollection`

Returns the ROC result.

---

## ROCP

```lua
ROCP(prices, period)
```

The Rate-of-Change Percentage (ROCP) is an alternative for ROC,
showing the values as percentages rather than absolutes. This indicator is usually plotted on its own chart.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `prices` | `HaasNumberCollection` | Yes | Source data. |
| `period` | `number` | Yes | Period length. |

**Returns:** `HaasNumberCollection`

Returns the ROCP result.

---

## ROCR

```lua
ROCR(prices, period)
```

The Rate-of-Change Ratio (ROCR) is an alternative for ROC,
showing the values as a ratio rather than absolutes. This indicator is usually plotted on its own chart.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `prices` | `HaasNumberCollection` | Yes | Source data. |
| `period` | `number` | Yes | Period length. |

**Returns:** `HaasNumberCollection`

Returns the ROCR result.

---

## ROCR100

```lua
ROCR100(prices, period)
```

The Rate-of-Change Ratio 100 Scale (ROCR100) is an alternative for ROC,
showing the values as a ratio rather than absolutes. This indicator is usually plotted on its own chart.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `prices` | `HaasNumberCollection` | Yes | Source data. |
| `period` | `number` | Yes | Period length. |

**Returns:** `HaasNumberCollection`

Returns the ROCR100 result.

---

## RSI

```lua
RSI(prices, period)
```

The Relative Strength Index (RSI), developed by J. Welles Wilder,
is a momentum oscillator that measures the speed and change of price movements.
The RSI oscillates between zero and 100. Traditionally the RSI is considered
overbought when above 70 and oversold when below 30, but can also be used to confirm
trends. This indicator is usually plotted on its own chart.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `prices` | `HaasNumberCollection` | Yes | Source data. |
| `period` | `number` | Yes | Period length. |

**Returns:** `HaasNumberCollection`

Returns the RSI result.

---

## RSI_ALTB

```lua
RSI_ALTB(prices, period)
```

The Relative Strength Index (RSI_ALTB), developed by J. Welles Wilder,
is a momentum oscillator that measures the speed and change of price movements.
The RSI oscillates between zero and 100. Traditionally the RSI is considered
overbought when above 70 and oversold when below 30, but can also be used to confirm
trends. This indicator is usually plotted on its own chart.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `prices` | `HaasNumberCollection` | Yes | Source data. |
| `period` | `number` | Yes | Period length. |

**Returns:** `HaasNumberCollection`

Returns the RSI_ALTB result.

---

## SAR

```lua
SAR(high, low, acceleration, maxSpeed)
```

The Parabolic Stop-And-Reverse (SAR), developed by Welles Wilder,
refers to a price-and-time-based trading system. Wilder called this the "Parabolic Time/Price System."
SAR stands for "stop and reverse", which is the actual indicator used in the system.
SAR trails price as the trend extends over time. The indicator is below prices as they're rising
and above prices as they're falling. In this regard, the indicator stops and reverses when the price
trend reverses and breaks above or below the indicator.
This indicator is usually plotted on the price chart.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `high` | `HaasNumberCollection` | Yes | High prices array. |
| `low` | `HaasNumberCollection` | Yes | Low prices array. |
| `acceleration` | `number` | Yes | Acceleration amount. |
| `maxSpeed` | `number` | Yes | Maximum speed. |

**Returns:** `HaasNumberCollection`

Returns the SAR result.

---

## SAREXT

```lua
SAREXT(high, low, startValue, offsetOnReverse, accelInitLong, accelLong, accelMaxLong, accelInitShort, accelShort, accelMaxShort)
```

The Parabolic Stop-And-Reverse [Extended] (SAREXT) is the same indicator as SAR(),
but opens up more parameters for the user to modify.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `high` | `HaasNumberCollection` | Yes | High prices array. |
| `low` | `HaasNumberCollection` | Yes | Low prices array. |
| `startValue` | `number` | Yes | Starting value. |
| `offsetOnReverse` | `number` | Yes | Offset amount on reverse point. |
| `accelInitLong` | `number` | Yes | Initial acceleration for Long position. |
| `accelLong` | `number` | Yes | Acceleration amount for Long position. |
| `accelMaxLong` | `number` | Yes | Maximum acceleration for Long position. |
| `accelInitShort` | `number` | Yes | Initial acceleration for Short position. |
| `accelShort` | `number` | Yes | Acceleration amount for Short position. |
| `accelMaxShort` | `number` | Yes | Maximum acceleration for Short position. |

**Returns:** `HaasNumberCollection`

Returns the SAREXT result.

---

## SlowRSI

```lua
SlowRSI(prices, period, signal)
```

The SlowRSI is a custom implementation from Haasonline. It uses a custom RSI and EMA calculation.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `prices` | `HaasNumberCollection` | Yes | Source data. |
| `period` | `number` | Yes | Period length. |
| `signal` | `number` | Yes | Signal length. |

**Returns:** `dynamic`

Returns the SlowRSI result.

---

## SMA

```lua
SMA(prices, period)
```

The Simple Moving Average (SMA) is an arithmetic moving average
calculated by adding recent closing prices and then dividing that by the number of time
periods in the calculation average.
This indicator is usually plotted on the price chart.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `prices` | `HaasNumberCollection` | Yes | Source data. |
| `period` | `number` | Yes | Period length. |

**Returns:** `HaasNumberCollection`

Returns the SMA result.

---

## SMA2

```lua
SMA2(prices, period)
```

The Simple Moving Average (SMA2) is an arithmetic moving average
calculated by adding recent closing prices and then dividing that by the number of time
periods in the calculation average.
This indicator is usually plotted on the price chart.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `prices` | `HaasNumberCollection` | Yes | Source data. |
| `period` | `number` | Yes | Period length. |

**Returns:** `HaasNumberCollection`

Returns the SMA2 result.

---

## SSTOCH

```lua
SSTOCH(prices, period)
```

The Simple Stochastic (SSTOCH)

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `prices` | `HaasNumberCollection` | Yes | Source data. |
| `period` | `number` | Yes | Period length. |

**Returns:** `HaasNumberCollection`

Returns the SSTOCH result.

---

## STDDEV

```lua
STDDEV(prices, period, deviation)
```

The Standard Deviation (STDDEV) is an indicator that measures
the size of recent price moves of an asset, to predict how volatile the price may be in
future. It can help you decide whether the volatility of the price is likely to increase
or decrease.
This indicator is usually plotted on its own chart.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `prices` | `HaasNumberCollection` | Yes | Source data. |
| `period` | `number` | Yes | Period length. |
| `deviation` | `number` | Yes | Amount of deviation. |

**Returns:** `HaasNumberCollection`

Returns the STDDEV result.

---

## STOCH

```lua
STOCH(high, low, close, fastK, slowK, slowD)
```

The Stochastic Oscillator [Full] (STOCH), developed by Dr. George Lane,
is a momentum indicator that uses support and resistance levels. The term stochastic
refers to the point of a current price in relation to its price range over a period of time.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `high` | `HaasNumberCollection` | Yes | High prices array. |
| `low` | `HaasNumberCollection` | Yes | Low prices array. |
| `close` | `HaasNumberCollection` | Yes | Close prices array. |
| `fastK` | `number` | Yes | Fast %K period length. |
| `slowK` | `number` | Yes | Slow %K period length. |
| `slowD` | `number` | Yes | Slow %D period length. |

**Returns:** `dynamic`

Returns the STOCH result.

---

## STOCHF

```lua
STOCHF(high, low, close, fastK, fastD, maType)
```

The Stochastic Oscillator [Fast] (STOCHF).

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `high` | `HaasNumberCollection` | Yes | High prices array. |
| `low` | `HaasNumberCollection` | Yes | Low prices array. |
| `close` | `HaasNumberCollection` | Yes | Close prices array. |
| `fastK` | `number` | Yes | Fast %K period length. |
| `fastD` | `number` | Yes | Fast %D period length. |
| `maType` | `enum` | Yes | MA type. |

**Returns:** `dynamic`

Returns the STOCHF result.

---

## STOCHRSI

```lua
STOCHRSI(prices, rsiPeriod, fastK, fastD, maType)
```

The Stochastic-RSI (STOCHRSI) is essentially an indicator of an indicator.
The Stochastic RSI is an oscillator that calculates a value between 0 and 1 which is
then plotted as a line. This indicator is primarily used for identifying overbought and oversold conditions.
This indicator is usually plotted on its own chart.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `prices` | `HaasNumberCollection` | Yes | Source data. |
| `rsiPeriod` | `number` | Yes | Period length. |
| `fastK` | `number` | Yes | Fast %K period length. |
| `fastD` | `number` | Yes | Fast %D period length. |
| `maType` | `enum` | Yes | The type of Moving Average. |

**Returns:** `dynamic`

Returns the STOCHRSI result.

---

## T3

```lua
T3(prices, period, vFactor)
```

The Triple Exponential Moving Average (T3), developed by Tim Tillson,
attempts to offers a moving average with better smoothing than traditional exponential moving average.
This indicator is usually plotted on the price chart.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `prices` | `HaasNumberCollection` | Yes | Source data. |
| `period` | `number` | Yes | Period length. |
| `vFactor` | `number` | Yes | Volume factor between 0 and 1. |

**Returns:** `HaasNumberCollection`

Returns the T3 result.

---

## TEMA

```lua
TEMA(prices, period)
```

The Triple Exponential Moving Average (TEMA), developed by Patrick Mulloy,
offers a moving average with less lag then traditional exponential moving average.
This indicator is usually plotted on the price chart.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `prices` | `HaasNumberCollection` | Yes | Source data. |
| `period` | `number` | Yes | Period length. |

**Returns:** `HaasNumberCollection`

Returns the TEMA result.

---

## TRANGE

```lua
TRANGE(high, low, close)
```

The True Range (TRANGE) developed by J. Welles Wilder,
is a technical analysis volatility indicator. The indicator does not provide an
indication of price trend, simply the degree of price volatility.
This indicator is usually plotted on its own chart.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `high` | `HaasNumberCollection` | Yes | High prices array. |
| `low` | `HaasNumberCollection` | Yes | Low prices array. |
| `close` | `HaasNumberCollection` | Yes | Close prices array. |

**Returns:** `HaasNumberCollection`

Returns the TRANGE result.

---

## TRIMA

```lua
TRIMA(prices, period)
```

The Triangular Moving Average (TRIMA) is similar to other
moving averages in that it shows the average (or mean) price over a specified number
of data points (usually a number of price bars). However, the triangular moving average
differs in that it is double smoothed — which also means it is averaged twice.
This indicator is usually plotted on the price chart.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `prices` | `HaasNumberCollection` | Yes | Source data. |
| `period` | `number` | Yes | Period length. |

**Returns:** `HaasNumberCollection`

Returns the TRIMA result.

---

## TRIX

```lua
TRIX(prices, period)
```

TRIX is a momentum oscillator that displays the percent rate of change of a triple exponentially smoothed moving average.
This indicator is usually plotted on its own chart.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `prices` | `HaasNumberCollection` | Yes | Source data. |
| `period` | `number` | Yes | Period length. |

**Returns:** `HaasNumberCollection`

Returns the TRIX result.

---

## TSF

```lua
TSF(prices, period)
```

Time Series Forecast (TSF) is a linear
 regression calculation that plots each bar’s current regression
 value using the least square fit method. This indicator is sometimes
 referred to as a moving linear regression similar to a moving average.
 For example, the TSF value that covers 10 days will have the same
 value as a 10-day Time Series Forecast. This differs slightly from
 the Linear Regression indicator in that the Linear Regression indicator
 does not add the slope to the ending value of the regression line.
This indicator is usually plotted on the price chart.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `prices` | `HaasNumberCollection` | Yes | Source data. |
| `period` | `number` | Yes | Period length. |

**Returns:** `HaasNumberCollection`

Returns the TSF result.

---

## TSI

```lua
TSI(prices, periodA, periodB)
```

The True Strength Index (TSI) is a momentum oscillator based on a double smoothing of price changes.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `prices` | `HaasNumberCollection` | Yes | Source data. |
| `periodA` | `number` | Yes | First smoothing period length. |
| `periodB` | `number` | Yes | Second smoothing period length. |

**Returns:** `HaasNumberCollection`

Returns the TSI result.

---

## TYPPRICE

```lua
TYPPRICE(high, low, close)
```

Typical Price (TYPPRICE) is simply an average of asset's price.
The Median Price (MEDPRICE) and Weighted Close Price (WCLPRICE) are similar indicators.
The Typical Price indicator provides a simple, single-line plot of the asset's average price.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `high` | `HaasNumberCollection` | Yes | High prices array. |
| `low` | `HaasNumberCollection` | Yes | Low prices array. |
| `close` | `HaasNumberCollection` | Yes | Close prices array. |

**Returns:** `HaasNumberCollection`

Returns the TYPPRICE result.

---

## UDRSI

```lua
UDRSI(prices, period)
```

The UpDown RSI (UDRSI) is an RSI Based Indicator that uses
an alternative formula to calculate the momentum oscillator. This Indicator works exactly like
RSI but instead of determining this from the difference in price, it is calculated by what
is called trend stepping. Trend stepping basically measures the closing price of a range of
candles and when the price of one candle is lower than the next, it is calculated as 1 price
step up. Because of this alternative calculation of Relative Strength Index, Up Down RSI
produces some unique Trade Signals.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `prices` | `HaasNumberCollection` | Yes | Source data. |
| `period` | `number` | Yes | Period length. |

**Returns:** `HaasNumberCollection`

Returns the UDRSI result.

---

## ULTOSC

```lua
ULTOSC(high, low, close, periodA, periodB, periodC)
```

The Ultimate Oscillator (ULTOSC), developed by Larry Williams,
is a momentum oscillator designed to capture momentum across three different timeframes.
The multiple timeframe objective seeks to avoid the pitfalls of other oscillators. Many
momentum oscillators surge at the beginning of a strong advance, only to form a bearish
divergence as the advance continues. This is because they are stuck with one timeframe.
The Ultimate Oscillator attempts to correct this fault by incorporating longer timeframes
into the basic formula. Williams identified a buy signal a based on a bullish divergence
and a sell signal based on a bearish divergence.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `high` | `HaasNumberCollection` | Yes | High prices array. |
| `low` | `HaasNumberCollection` | Yes | Low prices array. |
| `close` | `HaasNumberCollection` | Yes | Close prices array. |
| `periodA` | `number` | Yes | Fast period length. |
| `periodB` | `number` | Yes | Mid period length. |
| `periodC` | `number` | Yes | Slow period length. |

**Returns:** `HaasNumberCollection`

Returns the ULTOSC result.

---

## VAR

```lua
VAR(prices, period, deviation)
```

Variance (VAR), developed by Dr. Rene Koch,
is the variance ratio indicator that measures the degree of mean reversion or
trendiness in a time series. It is an easy and fast way to detect whether
a price series is trending, mean reverting or following a random walk.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `prices` | `HaasNumberCollection` | Yes | Source data. |
| `period` | `number` | Yes | Period length. |
| `deviation` | `number` | Yes | The amount of deviation. |

**Returns:** `HaasNumberCollection`

Returns the VAR result.

---

## WCLPRICE

```lua
WCLPRICE(high, low, close)
```

Weighted Close Price (WCLPRICE) is simply an average of an asset's price.
It gets its name from the fact that extra weight is given to the closing price.
The Median Price (MEDPRICE) and Typical Price (TYPPRICE) are similar indicators.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `high` | `HaasNumberCollection` | Yes | High prices array. |
| `low` | `HaasNumberCollection` | Yes | Low prices array. |
| `close` | `HaasNumberCollection` | Yes | Close prices array. |

**Returns:** `HaasNumberCollection`

Returns the WCLPRICE result.

---

## WILLR

```lua
WILLR(high, low, close, period)
```

Williams %R (WILLR), also known as the Williams Percent Range,
is a type of momentum indicator that moves between 0 and -100 and measures overbought
and oversold levels. The Williams %R may be used to find entry and exit points in the market.
This indicator is usually plotted on its own chart.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `high` | `HaasNumberCollection` | Yes | High prices array. |
| `low` | `HaasNumberCollection` | Yes | Low prices array. |
| `close` | `HaasNumberCollection` | Yes | Close prices array. |
| `period` | `number` | Yes | Period length. |

**Returns:** `HaasNumberCollection`

Returns the WILLR result.

---

## WiMA

```lua
WiMA(prices, period)
```

Wilder Moving Average (WiMA), also called Wilder's Smoothed Moving Average,
is similar to the Exponential Moving Average. Compared to other moving averages,
Wilder's MA responds more slowly to price changes, where an n-period Wilder MA gives
similar values to a 2n- period EMA. For example, a 14-period EMA has almost the
same values as a 7-period Wilder MA.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `prices` | `HaasNumberCollection` | Yes | Source data. |
| `period` | `number` | Yes | Period length. |

**Returns:** `HaasNumberCollection`

Returns the WiMA result.

---

## WMA

```lua
WMA(prices, period)
```

The Weighted Moving Average (WMA) puts more weight on recent
data and less on past data. This is done by multiplying each bar's price by
a weighting factor. Because of its unique calculation, WMA will follow prices
more closely than a corresponding Simple Moving Average.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `prices` | `HaasNumberCollection` | Yes | Source data. |
| `period` | `number` | Yes | Period length. |

**Returns:** `HaasNumberCollection`

Returns the WMA result.

---

## WWS

```lua
WWS(prices, period)
```

Welles Wilder's Smoothing (WWS), developed by J. Welles Wilder, Jr. and
is part of the Wilder's RSI indicator implementation. This indicator smooths price
movements to help you identify and spot bullish and bearish trends.
This indicator is usually plotted on the price chart.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `prices` | `HaasNumberCollection` | Yes | Source data. |
| `period` | `number` | Yes | Period length. |

**Returns:** `HaasNumberCollection`

Returns the WWS result.

---

## ZLEMA

```lua
ZLEMA(prices, period)
```

The Zero-Lag Exponential Moving Average (ZLEMA), developed by John Ehlers and Ric Way.
As is the case with the double exponential moving average (DEMA) and the triple exponential moving average (TEMA)
and as indicated by the name, the aim is to eliminate the inherent lag associated to all trend following
indicators which average a price over time. The idea is do a regular exponential moving average (EMA) calculation
but on a de-lagged data instead of doing it on the regular data. Data is de-lagged by removing the data from "lag"
days ago thus removing (or attempting to) the cumulative effect of the moving average.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `prices` | `HaasNumberCollection` | Yes | Source data. |
| `period` | `number` | Yes | Period length. |

**Returns:** `HaasNumberCollection`

Returns the ZLEMA result.

---

## ZLMA

```lua
ZLMA(prices, period, [maType], [maPeriod1], [maPeriod2])
```

The Zero Lag Moving Average (ZLMA) is the same as ZLEMA, but opens up the possibility to change the
type of the moving average. The idea is do a regular moving average (MA) calculation but on a de-lagged data instead of doing
it on the regular data. Data is de-lagged by removing the data from "lag" days ago thus removing (or attempting to) the
cumulative effect of the moving average.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `prices` | `HaasNumberCollection` | Yes | Source data. |
| `period` | `number` | Yes | Period length. |
| `maType` | `enum` | No | The type of Moving Average. |
| `maPeriod1` | `number` | No | Additional parameter used with 'kama' and 'mama' types. (1) |
| `maPeriod2` | `number` | No | Additional parameter used with 'kama' and 'mama' types. (2) |

**Returns:** `dynamic`

Returns the ZLMA result in an array.

---


# Technical Analysis Helpers

## CrossOver

```lua
CrossOver(value1, value2)
```

Checks if a crossover occurs between value1 and value2.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `value1` | `number|table` | Yes | Source value(s) 1. |
| `value2` | `number|table` | Yes | Source value(s) 2. |

**Returns:** `boolean`

Returns true if a crossover occurs between value1 and value2. Otherwise false.

---

## CrossOverSince

```lua
CrossOverSince(value1, value2)
```

Calculates the number of ticks since the last crossover.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `value1` | `number|table` | Yes | Source value(s) 1. |
| `value2` | `number|table` | Yes | Source value(s) 2. |

**Returns:** `number`

Returns the number of ticks since the last crossover. Return value of 1 represents the crossunder happening 'now'.

---

## CrossUnder

```lua
CrossUnder(value1, value2)
```

Checks if a crossunder occurs between value1 and value2.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `value1` | `number|table` | Yes | Source value(s) 1. |
| `value2` | `number|table` | Yes | Source value(s) 2. |

**Returns:** `boolean`

Returns true if a crossunder occurs between value1 and value2. Otherwise false.

---

## CrossUnderSince

```lua
CrossUnderSince(value1, value2)
```

Calculates the number of ticks since the last crossunder.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `value1` | `number|table` | Yes | Source value(s) 1. |
| `value2` | `number|table` | Yes | Source value(s) 2. |

**Returns:** `number`

Returns the number of ticks since the last crossunder. Return value of 1 represents the crossunder happening 'now'.

---

## GetSwing

```lua
GetSwing(input1, input2)
```

Calculates the amount of swing in percentages.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input1` | `number|table` | Yes | Source value(s) 1. |
| `input2` | `number|table` | Yes | Source value(s) 2. |

**Returns:** `HaasNumberCollection`

Returns the amount of swing in percentages.

---

## IsFalling

```lua
IsFalling(array, lookback)
```

Checks if the values are decreasing towards the newest within the lookback period.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `array` | `HaasNumberCollection` | Yes | Source values. |
| `lookback` | `number` | Yes | Lookback value. |

**Returns:** `boolean`

Returns true if the values are decreasing towards the newest within the lookback period. Otherwise false.

---

## IsFallingSince

```lua
IsFallingSince(array, lookback)
```

Counts the number of falling ticks.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `array` | `HaasNumberCollection` | Yes | Source values. |
| `lookback` | `number` | Yes | Lookback value. |

**Returns:** `number`

Returns the number of ticks the data is falling.

---

## IsRising

```lua
IsRising(array, lookback)
```

Checks if the values are increasing towards the newest within the lookback period.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `array` | `HaasNumberCollection` | Yes | Source values. |
| `lookback` | `number` | Yes | Lookback value. |

**Returns:** `boolean`

Returns true if the values are increasing towards the newest within the lookback period. Otherwise false.

---

## IsRisingSince

```lua
IsRisingSince(array, lookback)
```

Counts the number of rising ticks.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `array` | `HaasNumberCollection` | Yes | Source values. |
| `lookback` | `number` | Yes | Lookback value. |

**Returns:** `number`

Returns the number of ticks the data is rising.

---


*Auto-generated from HaasScript source code.*
