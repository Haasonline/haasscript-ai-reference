# Helpers & Utilities

121 commands across 7 categories.

## Categories

- [Array Helpers](#array-helpers) (22 commands)
- [Equations](#equations) (15 commands)
- [Flow Control](#flow-control) (5 commands)
- [Mathematical](#mathematical) (49 commands)
- [Memory Helpers](#memory-helpers) (11 commands)
- [Miscellaneous](#miscellaneous) (9 commands)
- [String Helpers](#string-helpers) (10 commands)

---

# Array Helpers

## ArrayAdd

```lua
ArrayAdd(input, element)
```

Adds an element to an existing array.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `dynamic` | Yes | Source data. |
| `element` | `dynamic` | Yes | Element which to add to the array. |

**Returns:** `dynamic`

Returns the input array.

---

## ArrayAny

```lua
ArrayAny(input, [value])
```

**Aliases:** `ArrayContains`

Returns true if the array contains any element or an element matches a specific value.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `dynamic` | Yes | Source data. |
| `value` | `dynamic` | No | Optional value to match. |

**Returns:** `boolean`

Returns true if the input has any elements or if 'value' is defined when any of the elements matches the value.

---

## ArrayConcat

```lua
ArrayConcat(array1, array2)
```

Concatenates two arrays.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `array1` | `dynamic` | Yes | First array. |
| `array2` | `dynamic` | Yes | Second array. |

**Returns:** `dynamic`

Returns the concatenated array.

---

## ArrayDistinct

```lua
ArrayDistinct(input)
```

Returns a new list with only unique elements. In other words; removes all duplicates.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `dynamic` | Yes | Source data. |

**Returns:** `dynamic`

Returns the unique values of the input.

---

## ArrayFilter

```lua
ArrayFilter(input, value, [filterType])
```

Returns a new list with elements that only matches the value. Matching-rules specified by filterType.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `dynamic` | Yes | Source data. |
| `value` | `dynamic` | Yes | Value to match. |
| `filterType` | `enum` | No | The type of filter to use. Default is ArrayFilterInclusiveType. |

**Returns:** `dynamic`

Returns the list.

---

## ArrayFind

```lua
ArrayFind(input, value, [filterType])
```

Returns a new list of indices of the matching values. Matching-rules specified by filterType.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `dynamic` | Yes | Source data. |
| `value` | `dynamic` | Yes | Value to match. |
| `filterType` | `enum` | No | The type of filter to use. Default is ArrayFilterInclusiveType. |

**Returns:** `dynamic`

Returns the list.

---

## ArrayIndex

```lua
ArrayIndex(input, index)
```

**Aliases:** `ArrayGet`

Gets the value on a specific index. A negative index will take from the end of the array.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `dynamic` | Yes | Source data. |
| `index` | `number` | Yes | The index to get. |

**Returns:** `dynamic`

Return the value on that index.

---

## ArrayIndexField

```lua
ArrayIndexField(input, index)
```

Gets the value on a specific index.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `dynamic` | Yes | Source data. |
| `index` | `number` | Yes | The index to get. |

**Returns:** `dynamic`

Return the value on that index.

---

## ArrayLast

```lua
ArrayLast(input, [offset])
```

Gets the last value of the array with an optional offset.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `dynamic` | Yes | Source data. |
| `offset` | `number` | No | The offset from the last index. |

**Returns:** `dynamic`

Return the last value of the array taking the offset into account.

---

## ArrayPop

```lua
ArrayPop(input)
```

Remove an item from the end of an array.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `dynamic` | Yes | Source data. |

**Returns:** `dynamic`

Returns the input array.

---

## ArrayRemove

```lua
ArrayRemove(input, index)
```

Removes an element from a specified index.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `dynamic` | Yes | Source data. |
| `index` | `number` | Yes | Index of the element to remove. |

**Returns:** `dynamic`

Returns the input array.

---

## ArrayReplace

```lua
ArrayReplace(input, oldValue, newValue)
```

Replaces a specific value with another value.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `dynamic` | Yes | Source data. |
| `oldValue` | `dynamic` | Yes | The old value. |
| `newValue` | `dynamic` | Yes | The new value. |

**Returns:** `dynamic`

The input with replaced values.

---

## ArrayShift

```lua
ArrayShift(input)
```

Remove an item from the beginning of an array.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `dynamic` | Yes | Source data. |

**Returns:** `dynamic`

The input without the first value

---

## ArraySort

```lua
ArraySort(input, [descending])
```

Sorts the array values in ascending or descending order.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `dynamic` | Yes | Source data. |
| `descending` | `boolean` | No | The order of the sort. If set to true, order will be descending. Otherwise ascending. |

**Returns:** `dynamic`

Returns the sorted array.

---

## ArraySum

```lua
ArraySum(input, [key])
```

Computes the sum of the array. Can be used in combination with a list of numbers or container results like GetAllOpenOrders() or GetAllFinishedOrders().

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `dynamic` | Yes | Source data. |
| `key` | `string` | No | The key of the data to be summed. |

**Returns:** `number`

Returns the sum of the array.

---

## ArrayUnshift

```lua
ArrayUnshift(input, element)
```

Adds an element to the beginning of an array.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `dynamic` | Yes | Source data. |
| `element` | `dynamic` | Yes | Element which to add to the array. |

**Returns:** `dynamic`

The input with the element added in front of the array

---

## Count

```lua
Count(input, [value])
```

Gets the length of an array or the number of occurrences within the array of a specific value.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `dynamic` | Yes | Source data. |
| `value` | `dynamic` | No | Optional value to match. |

**Returns:** `number`

Returns the length of an array or the number of occurrences within the array of a specific value.

---

## Grab

```lua
Grab(input, offset, [count])
```

**Aliases:** `Range`

Grabs a range from the array. If count is not set, return all after the offset.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `dynamic` | Yes | Source data. |
| `offset` | `number` | Yes | Offset from start. |
| `count` | `number` | No | Number of items to grab. |

**Returns:** `dynamic`

Specified range from the array.

---

## NewArray

```lua
NewArray([elements[]])
```

**Aliases:** `NewList`

Creates an array.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `elements[]` | `...dynamic` | No | Optional array element. |

**Returns:** `dynamic`

Returns an array with the input elements.

---

## NewSortedArray

```lua
NewSortedArray([elements1], [elements2], [elements3], [elements4], [elements5], [elements6], [elements7], [elements8], [elements9], [elements10])
```

**Aliases:** `NewSortedList`

Creates a sorted array.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `elements1` | `dynamic` | No | Optional array element 1. |
| `elements2` | `dynamic` | No | Optional array element 2. |
| `elements3` | `dynamic` | No | Optional array element 3. |
| `elements4` | `dynamic` | No | Optional array element 4. |
| `elements5` | `dynamic` | No | Optional array element 5. |
| `elements6` | `dynamic` | No | Optional array element 6. |
| `elements7` | `dynamic` | No | Optional array element 7. |
| `elements8` | `dynamic` | No | Optional array element 8. |
| `elements9` | `dynamic` | No | Optional array element 9. |
| `elements10` | `dynamic` | No | Optional array element 10. |

**Returns:** `dynamic`

Returns an sorted array with the input elements.

---

## Offset

```lua
Offset(input, offset)
```

Gets an offset amount of the given array.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `dynamic` | Yes | Source data. |
| `offset` | `number` | Yes | Number of record to remove. |

**Returns:** `dynamic`

Returns the given array with the offset amount of data removed from it.

---

## SourceManager

```lua
SourceManager(newValue, [interval], [cap], [initialValues])
```

Stores numeric values in a collection. Only adds the new value when the candle on the specified interval closed. This results in interval based data.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `newValue` | `number` | Yes | This value will be added in front of the collection when the candle closed. |
| `interval` | `number` | No | The interval on which the data is based. Default is the selected main interval. |
| `cap` | `number` | No | Maximum size of the collection. |
| `initialValues` | `HaasNumberCollection` | No | The initial values when source in initialized |

**Returns:** `HaasNumberCollection`

Returns the numeric collection.

---


# Equations

## Compare

```lua
Compare(input1, input2)
```

Checks if input1 is below, equal to or above input2.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input1` | `number|table` | Yes | Source value(s) 1. |
| `input2` | `number|table` | Yes | Source value(s) 2. |

**Returns:** `unknown`

Returns a list with booleans.

---

## Equals

```lua
Equals(value1, value2)
```

Compares two values and returns whether or not the values are equal.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `value1` | `dynamic` | Yes | First value to compare. |
| `value2` | `dynamic` | Yes | Second value to compare. |

**Returns:** `boolean`

Returns true when the values are equal.

---

## False

```lua
False()
```

Creates a false value.

**Returns:** `boolean`

Returns false.

---

## IfNull

```lua
IfNull(value, defaultValue)
```

Sets the defaultValue if the value is nil.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `value` | `dynamic` | Yes | The value to compare. |
| `defaultValue` | `dynamic` | Yes | The default value when the value is not set. |

**Returns:** `dynamic`

Returns the value of defaultValue when value is nil.

---

## IsBiggerAndSmallerThan

```lua
IsBiggerAndSmallerThan(value, lowValue, highValue)
```

Checks if the value is bigger than lowValue and smaller then highValue.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `value` | `number|table` | Yes | The value to compare. |
| `lowValue` | `number` | Yes | The lowest value. |
| `highValue` | `number` | Yes | The highest value. |

**Returns:** `boolean`

Returns true the value is bigger than lowValue and smaller then highValue.

---

## IsBiggerOrSmallerThan

```lua
IsBiggerOrSmallerThan(value, highValue, lowValue)
```

Checks if the value is bigger than highValue or smaller then lowValue.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `value` | `number|table` | Yes | The value to compare. |
| `highValue` | `number` | Yes | The highest value. |
| `lowValue` | `number` | Yes | The lowest value. |

**Returns:** `boolean`

Returns true the value is bigger than highValue or smaller then lowValue.

---

## IsBiggerThan

```lua
IsBiggerThan(input1, input2)
```

Checks if input1 is bigger than input2.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input1` | `number|table` | Yes | The value to compare. |
| `input2` | `number` | Yes | The value to compare with. |

**Returns:** `boolean`

Returns true input1 is bigger than input2.

---

## IsFalse

```lua
IsFalse(values[])
```

**Aliases:** `Not`

Validates all values on being false.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `values[]` | `...boolean` | Yes | The boolean to compare. |

**Returns:** `boolean`

Returns true all values are false.

---

## IsNotNull

```lua
IsNotNull(value)
```

Checks if the value is not null.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `value` | `dynamic` | Yes | The value to compare. |

**Returns:** `boolean`

Returns true if the value is not null.

---

## IsNull

```lua
IsNull(value)
```

Checks if the value is null.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `value` | `dynamic` | Yes | The value to compare. |

**Returns:** `boolean`

Returns true if the value is null.

---

## IsSmallerThan

```lua
IsSmallerThan(input1, input2)
```

Checks if input1 is smaller than input2.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input1` | `number|table` | Yes | The value to compare. |
| `input2` | `number` | Yes | The value to compare with. |

**Returns:** `boolean`

Returns true if input1 is smaller than input2.

---

## IsTrue

```lua
IsTrue(values[])
```

**Aliases:** `And`

Validates all values on being true.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `values[]` | `...boolean` | Yes | The boolean to compare. |

**Returns:** `boolean`

Returns true all values are true.

---

## NotEquals

```lua
NotEquals(value1, value2)
```

Compares two values and returns whether or not the values are different.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `value1` | `dynamic` | Yes | First value to compare. |
| `value2` | `dynamic` | Yes | Second value to compare. |

**Returns:** `boolean`

Returns true when the values are not equal.

---

## Or

```lua
Or(values[])
```

Validates if any of the values is true.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `values[]` | `...boolean` | Yes | The boolean to compare. |

**Returns:** `boolean`

Returns true if any of the all values is true.

---

## True

```lua
True()
```

Creates a true value.

**Returns:** `boolean`

Returns true.

---


# Flow Control

## Branch

```lua
Branch(input)
```

Validates the input boolean and sets the output result accordingly. This block can be useful in decision-based flows.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `boolean` | Yes | Input boolean to check. |

**Returns:** `boolean`

Returns a list of boolean results.

---

## DelayExecution

```lua
DelayExecution(timestamp, delay)
```

Delays an execution by x minutes

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `timestamp` | `number` | Yes | The start time of the delay. |
| `delay` | `number` | Yes | The delay in minutes. |

**Returns:** `boolean`

Returns false when the delay is active. True when it is passed.

---

## IfElse

```lua
IfElse(statement, value1, value2)
```

**Aliases:** `Switch`

Will return 1 of 2 values based on the input boolean.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `statement` | `boolean` | Yes | Input boolean to check. |
| `value1` | `dynamic` | Yes | If true, this value is passed on. |
| `value2` | `dynamic` | Yes | If false, this value is passed on. |

**Returns:** `dynamic`

Return the first value if true or the second value when false.

---

## IfElseIf

```lua
IfElseIf(statement1, statement2, value1, value2, value3)
```

IfElseIf returns a value based on the boolean inputs.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `statement1` | `boolean` | Yes | Statement one. |
| `statement2` | `boolean` | Yes | Statement two. |
| `value1` | `dynamic` | Yes | Value1, returns if statement1 is valid. |
| `value2` | `dynamic` | Yes | Value2, returns if statement2 is valid. |
| `value3` | `dynamic` | Yes | Value3, returns is statement 1 and 2 are invalid. |

**Returns:** `dynamic`

Return the first value if statement1 is true, return the second value if statement2 is true, or returns value3 when all statements are invalid.

---

## Merge

```lua
Merge([values[]])
```

This commands executes at the very last. When one of the input values is set, the execution will continue from this command.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `values[]` | `...dynamic` | No | Possible values for this flow. |

**Returns:** `dynamic`

Returns the first set input value or doesn't execute.

---


# Mathematical

## Abs

```lua
Abs(input)
```

Calculates an absolute/positive number.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `number|table` | Yes | Source value. |

**Returns:** `unknown`

Returns an absolute/positive number.

---

## Acos

```lua
Acos(input)
```

Calculates the angle whose cosine is the specified number.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `number|table` | Yes | A number representing a cosine, that must be greater than or equal to -1, but less than or equal to 1. |

**Returns:** `unknown`

Returns the angle whose cosine is the specified number.

---

## Add

```lua
Add(input1, [input2])
```

**Aliases:** `Sum`

Adds numbers/tables and returns the result number/table.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input1` | `number|table` | Yes | Source value(s) 1. |
| `input2` | `number|table` | No | Source value(s) 2. |

**Returns:** `unknown`

Returns the total sum of a single value is provide, adds the 2 values if there are 2 provided.

---

## AddPercentage

```lua
AddPercentage(value, percentage)
```

**Aliases:** `AddPerc`

Add a percentage to the value.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `value` | `number|table` | Yes | The main value. |
| `percentage` | `number` | Yes | The percentage to add. |

**Returns:** `HaasNumberCollection`

Returns the value with the added percentage.

---

## Asin

```lua
Asin(input)
```

Calculates the angle whose sine is the specified number.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `number|table` | Yes | Source value(s). |

**Returns:** `unknown`

Returns the angle whose sine is the specified number.

---

## Atan

```lua
Atan(input)
```

Calculates the angle whose tangent is the specified number.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `number|table` | Yes | A number representing a tangent. |

**Returns:** `unknown`

Returns the angle whose tangent is the specified number.

---

## Atan2

```lua
Atan2(input1, input2)
```

Calculates the angle whose tangent is the quotient of two specified numbers.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input1` | `number|table` | Yes | The y coordinate of a point. |
| `input2` | `number|table` | Yes | The x coordinate of a point. |

**Returns:** `unknown`

Returns the angle whose tangent is the quotient of two specified numbers.

---

## Average

```lua
Average(input1, [input2])
```

Calculates the average value of the array, two numbers or averages between 2 arrays.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input1` | `number|table` | Yes | Source value(s) 1. |
| `input2` | `number|table` | No | Source value(s) 2. |

**Returns:** `unknown`

Returns the average value of the array, two numbers or averages between 2 arrays.

---

## Average2

```lua
Average2(input, [period])
```

Calculates the average value of the array for a certain period.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `HaasNumberCollection` | Yes | Source values. |
| `period` | `number` | No | Period length. |

**Returns:** `unknown`

Returns the average value of the array for a certain period.

---

## Ceil

```lua
Ceil(input)
```

Calculates the smallest integral value greater than or equal to the specified number.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `number|table` | Yes | Source value(s). |

**Returns:** `unknown`

Returns the smallest integral value greater than or equal to the specified number.

---

## Change

```lua
Change(prices)
```

Calculates the absolute price changes between the values.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `prices` | `HaasNumberCollection` | Yes | Source data. |

**Returns:** `HaasNumberCollection`

Returns the new-old change of the values in an array.

---

## Clamp

```lua
Clamp(input, min, max)
```

Calculates the number or table of numbers clamped between the specified minimum and maximum.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `number|table` | Yes | Source value(s). |
| `min` | `number` | Yes | Minimum value. |
| `max` | `number` | Yes | Maximum value. |

**Returns:** `unknown`

Returns number or table of numbers clamped between the specified minimum and maximum.

---

## Compress

```lua
Compress(input)
```

Calculates the average values of every 2 values in an array. When the input is [10, 20, 30, 40] the result will be [15, 35].

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `HaasNumberCollection` | Yes | Source data. |

**Returns:** `HaasNumberCollection`

Returns the compressed array.

---

## Cos

```lua
Cos(input)
```

Calculates the cosine of the specified angle.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `number|table` | Yes | Source value(s). |

**Returns:** `unknown`

Returns the cosine of the specified angle.

---

## Cosh

```lua
Cosh(input)
```

Calculates the hyperbolic cosine of the specified angle.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `number|table` | Yes | Source value(s). |

**Returns:** `unknown`

Returns the hyperbolic cosine of the specified angle.

---

## Delta

```lua
Delta(input1, [input2])
```

**Aliases:** `PercentageChange`

Calculates the percentage between input1 and input2 or the percentage change between the values of input1.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input1` | `number|table` | Yes | Source value(s) 1. |
| `input2` | `number|table` | No | Source value(s) 2. |

**Returns:** `unknown`

Returns the percentage between input1 and input2. When for example input1 is 5 and input2 is 6, 20 is returned.

---

## Div

```lua
Div(input1, input2)
```

Divides input1 with input2. If one of the parameters is a single value, the other is divided by this.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input1` | `number|table` | Yes | Source value(s) 1. |
| `input2` | `number|table` | Yes | Source value(s) 2. |

**Returns:** `unknown`

Returns the divided numbers/tables.

---

## Exp

```lua
Exp(input)
```

Calculates 'e' raised to the specified power.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `number|table` | Yes | Source value(s). |

**Returns:** `unknown`

Returns 'e' raised to the specified power.

---

## FilterAbove

```lua
FilterAbove(input, threshold)
```

Filters out all values above a specific value.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `number|table` | Yes | Source data. |
| `threshold` | `number` | Yes | The maximum threshold value. |

**Returns:** `HaasNumberCollection`

Returns the filtered input.

---

## FilterBelow

```lua
FilterBelow(input, threshold)
```

Filters out all values below a specific value.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `number|table` | Yes | Source data. |
| `threshold` | `number` | Yes | The old value. |

**Returns:** `HaasNumberCollection`

Returns the filtered input.

---

## Floor

```lua
Floor(input)
```

Calculates the largest integral value less than or equal to the specified number.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `number|table` | Yes | Source value(s). |

**Returns:** `unknown`

Returns the largest integral value less than or equal to the specified number.

---

## GetHigh

```lua
GetHigh(prices, depth, [offset])
```

Gets the highest value.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `prices` | `HaasNumberCollection` | Yes | Source data. |
| `depth` | `number` | Yes | Number of records to include. |
| `offset` | `number` | No | Number of records to skip. |

**Returns:** `number`

Returns the highest value.

---

## GetHighs

```lua
GetHighs(array, depth)
```

Gets the highest values within lookback period.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `array` | `HaasNumberCollection` | Yes | Source data. |
| `depth` | `number` | Yes | Number of records to include. |

**Returns:** `HaasNumberCollection`

Returns the highest values within lookback period.

---

## GetLow

```lua
GetLow(prices, depth, [offset])
```

Gets the lowest value.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `prices` | `HaasNumberCollection` | Yes | Source data. |
| `depth` | `number` | Yes | Number of records to include. |
| `offset` | `number` | No | Number of records to skip. |

**Returns:** `number`

Returns the lowest value.

---

## GetLows

```lua
GetLows(array, depth)
```

Gets the lowest values within lookback period.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `array` | `HaasNumberCollection` | Yes | Source data. |
| `depth` | `number` | Yes | Number of records to include. |

**Returns:** `HaasNumberCollection`

Returns the lowest values within lookback period.

---

## Ln

```lua
Ln(input1, [input2])
```

Calculates the logarithm of a specified number.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input1` | `number|table` | Yes | Source value(s) 1. |
| `input2` | `number|table` | No | Source value(s) 2. |

**Returns:** `unknown`

Returns the logarithm of a specified number.

---

## Log10

```lua
Log10(input)
```

Calculates the base 10 logarithm of a specified number.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `number|table` | Yes | Source value(s). |

**Returns:** `unknown`

Returns the base 10 logarithm of a specified number.

---

## Max

```lua
Max(values[])
```

Returns the maximum value. If the input is a single array, it returns the biggest number in that array (single value). If inputs are both numbers, it returns the biggest number (single value). If inputs are array and number, it returns the biggest number (array[1] vs number, single value). If inputs are both arrays, it returns an array of the biggest numbers per cell (array, length determined by the shorter array).

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `values[]` | `NumberOrTableParams` | Yes | Source value(s). |

**Returns:** `unknown`

Returns the biggest number or an array of biggest numbers per cell.

---

## Min

```lua
Min(values[])
```

Returns the minimum value. If the input is a single array, it returns the minimum in that array (single value). If inputs are both numbers, it returns the smallest number (single value). If inputs are array and number, it returns the smallest number (single value). If inputs are both arrays, it returns an array of the smallest numbers per cell (array, length determined by shortest array)

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `values[]` | `NumberOrTableParams` | Yes | Source value(s). |

**Returns:** `unknown`

Returns the smallest number or an array of smallest numbers per cell.

---

## Mul

```lua
Mul(input1, input2)
```

**Aliases:** `Mult`

Multiplies input1 with input2. If one of the parameters is a single value, the other is multiplied by this.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input1` | `number|table` | Yes | Source value(s) 1. |
| `input2` | `number|table` | Yes | Source value(s) 2. |

**Returns:** `unknown`

Returns the multiplied numbers/tables.

---

## NumberMax

```lua
NumberMax()
```

Enumeration, returns the maximum number.

**Returns:** `number`

Returns a maximum value (2147483647).

---

## NumberMin

```lua
NumberMin()
```

Enumeration, returns the minimum number.

**Returns:** `number`

Returns a minimum value (-2147483648).

---

## PI

```lua
PI()
```

Enumeration, returns the number of PI.

**Returns:** `number`

Returns PI (3.14159265359).

---

## Pow

```lua
Pow(value, power)
```

Calculates a specified number/table raised to the specified power number/table.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `value` | `number|table` | Yes | The base value |
| `power` | `number|table` | Yes | The power value |

**Returns:** `unknown`

Returns a specified number/table raised to the specified power number/table.

---

## Random

```lua
Random([min], [max])
```

Creates a random number.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `min` | `number` | No | Minimum value. Default is 0. |
| `max` | `number` | No | Minimum value. Default is 1. |

**Returns:** `number`

Returns a random number.

---

## ReplaceAbove

```lua
ReplaceAbove(input, threshold, newValue)
```

Replaces any value above a specific value with another value.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `number|table` | Yes | Source data. |
| `threshold` | `number` | Yes | Values above this threshold will be replaced. |
| `newValue` | `number` | Yes | The new value. |

**Returns:** `HaasNumberCollection`

Returns the input with replaced values.

---

## ReplaceBelow

```lua
ReplaceBelow(input, threshold, newValue)
```

Replaces any value below a specific value with another value.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `number|table` | Yes | Source data. |
| `threshold` | `number` | Yes | Values below this threshold will be replaced. |
| `newValue` | `number` | Yes | The new value. |

**Returns:** `HaasNumberCollection`

Returns the input with replaced values.

---

## Round

```lua
Round(input, digits)
```

Rounds a value (or values if table) to the nearest integer or to the specified number of fractional digits.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `number|table` | Yes | Source value(s). |
| `digits` | `number` | Yes | Number of digits after decimal point. |

**Returns:** `unknown`

Returns the rounded number.

---

## Satoshi

```lua
Satoshi()
```

**Aliases:** `Epsilon`

Enumeration, returns 1 Satoshi (=Epsilon).

**Returns:** `number`

Returns 1 Satoshi (=Epsilon).

---

## Sd

```lua
Sd(input)
```

Calculates the Standard Deviation from the data series.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `HaasNumberCollection` | Yes | Source value(s). |

**Returns:** `number`

Returns the Standard Deviation.

---

## Sign

```lua
Sign(input)
```

Calculates an integer that indicates the sign of a number.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `number|table` | Yes | Source value(s). |

**Returns:** `unknown`

Returns an integer that indicates the sign of a number.

---

## Sin

```lua
Sin(input)
```

Calculates the sine of the specified angle.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `number|table` | Yes | Source value(s). |

**Returns:** `unknown`

Returns the sine of the specified angle.

---

## Sinh

```lua
Sinh(input)
```

Calculates the hyperbolic sine of the specified angle.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `number|table` | Yes | Source value(s). |

**Returns:** `unknown`

Returns the hyperbolic sine of the specified angle.

---

## Sqrt

```lua
Sqrt(input)
```

Calculates the square root of a specified number.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `number|table` | Yes | Source value(s). |

**Returns:** `unknown`

Returns the square root of a specified number.

---

## Sub

```lua
Sub(input1, input2)
```

Subtracts numbers/tables and returns the result number/table.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input1` | `number|table` | Yes | Source value(s) 1. |
| `input2` | `number|table` | Yes | Source value(s) 2. |

**Returns:** `unknown`

Calculates the subtracted numbers/tables.

---

## SubPercentage

```lua
SubPercentage(value, percentage)
```

**Aliases:** `SubPerc`

Subtracts a percentage to the value.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `value` | `number|table` | Yes | The main value. |
| `percentage` | `number` | Yes | The percentage to subtract. |

**Returns:** `HaasNumberCollection`

Returns the value with the subtracted percentage.

---

## Tan

```lua
Tan(input)
```

Calculates the tangent of the specified angle.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `number|table` | Yes | Source value(s). |

**Returns:** `unknown`

Returns the tangent of the specified angle.

---

## Tanh

```lua
Tanh(input)
```

Calculates the hyperbolic tangent of the specified angle.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `number|table` | Yes | Source value(s). |

**Returns:** `unknown`

Returns the hyperbolic tangent of the specified angle.

---

## Truncate

```lua
Truncate(input, digits)
```

Truncates a value (or values if table) to the nearest integer or to the specified number of fractional digits.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `number|table` | Yes | Source value(s). |
| `digits` | `number` | Yes | Number of digits after decimal point. |

**Returns:** `unknown`

Returns the truncated number.

---


# Memory Helpers

## Delete

```lua
Delete(key)
```

Deletes a value on a specific key.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `key` | `string` | Yes | Unique identifier. |

**Returns:** `void`

---

## InitExportData

```lua
InitExportData(name, headers)
```

Initialize a file that can be exported later.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `name` | `string` | Yes | Unique name of the file |
| `headers` | `...string` | Yes | Header definition |

**Returns:** `void`

---

## Load

```lua
Load(key, [defaultValue])
```

Loads a value saved on the specified key.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `key` | `string` | Yes | Unique identifier. |
| `defaultValue` | `dynamic` | No | Default value for when no data is saved on the key. Default is nil. |

**Returns:** `dynamic`

Returns the saved value or nil.

---

## Log

```lua
Log(message, [color], [prefix], [suffix])
```

Create a message in the log.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `message` | `dynamic` | Yes | Message to log. |
| `color` | `string` | No | The color of the log message. |
| `prefix` | `string` | No | Optional prefix for the log message |
| `suffix` | `string` | No | Optional suffix for the log message |

**Returns:** `void`

---

## LogError

```lua
LogError(message)
```

Create an error message in the log.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `message` | `dynamic` | Yes | Error to log. |

**Returns:** `void`

---

## LogWalletError

```lua
LogWalletError(message)
```

Create a warning message in the log and activates the trade amount error.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `message` | `dynamic` | Yes | Warning to log. |

**Returns:** `void`

---

## LogWarning

```lua
LogWarning(message)
```

Create a warning message in the log.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `message` | `dynamic` | Yes | Warning to log. |

**Returns:** `void`

---

## Save

```lua
Save(key, value)
```

Saves a value on a specific key. This value can be retrieved with Load(). A maximum of 10.000 keys can be stored.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `key` | `string` | Yes | Unique identifier. |
| `value` | `dynamic` | Yes | The value to store. |

**Returns:** `void`

---

## SessionGet

```lua
SessionGet(key)
```

Gets a value stored in the session memory.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `key` | `string` | Yes | The key of the saved data. |

**Returns:** `dynamic`

Returns a value stored in the session memory.

---

## SessionSet

```lua
SessionSet(key, value)
```

Stores a value for this session only.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `key` | `string` | Yes | The key of the saved data. |
| `value` | `dynamic` | Yes | The data to save. |

**Returns:** `void`

---

## WriteExportData

```lua
WriteExportData(name, values)
```

Write data to a file that can be exported later. File size is unlimited, use with caution.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `name` | `string` | Yes | Name of the file to write the data to. |
| `values` | `...string` | Yes | The values to write to the file. Needs to match the number of headers defined in InitExportData. |

**Returns:** `void`

---


# Miscellaneous

## Comment

```lua
Comment(value)
```

Creates a text area block for comments.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `value` | `string` | Yes | The comment value. |

**Returns:** `void`

---

## ConvertNull

```lua
ConvertNull(input, [value])
```

Converts any null value(s) to a specified value. By default, all null values are converted to 0.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `input` | `dynamic` | Yes | The input value. |
| `value` | `dynamic` | No | The value to convert nulls to. Default is 0. |

**Returns:** `dynamic`

Returns the value/array without nulls.

---

## CustomReport

```lua
CustomReport(key, value, [group], [displayValue])
```

Creates an item in the bots custom reports.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `key` | `string` | Yes | The name of the item. |
| `value` | `dynamic` | Yes | The value of the item. |
| `group` | `string` | No | The name of the report. By default its 'Custom Report'. |
| `displayValue` | `boolean` | No | When set on true, this value will be visible on the bot list in the UI.'. |

**Returns:** `void`

---

## GetCommand

```lua
GetCommand(name)
```

Gets a command by name.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `name` | `string` | Yes | The name of the command. |

**Returns:** `dynamic`

Returns the command.

---

## GetType

```lua
GetType(value)
```

Gets the data type of the input.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `value` | `dynamic` | Yes | The input value. |

**Returns:** `enum`

Returns the data type.

---

## HNC

```lua
HNC([size], [value])
```

HNC is an array on which match can be applied directly. This is not position with the lua code `{}`.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `size` | `number` | No | The size of the array. Default 0. |
| `value` | `number` | No | The default value when size is set. Default is 0. |

**Returns:** `HaasNumberCollection`

Returns an array with or without values.

---

## Number

```lua
Number(value)
```

Creates a block with a numeric input field.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `value` | `number` | Yes | The input value. |

**Returns:** `number`

Returns the input value.

---

## Return

```lua
Return()
```

Stops the current update cycle. If used in a command script, it will only stop the execution of the current command, not the main script.

**Returns:** `void`

---

## Text

```lua
Text(value)
```

Creates a block with an input field.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `value` | `string` | Yes | The input value. |

**Returns:** `string`

Returns the input value.

---


# String Helpers

## NewGuid

```lua
NewGuid()
```

Creates an unique identifier.

**Returns:** `string`

Returns the unique identifier as string.

---

## Parse

```lua
Parse(value, type)
```

Converts the value to a specific type.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `value` | `dynamic` | Yes | The something. |
| `type` | `enum` | Yes | The parameter type enum. |

**Returns:** `dynamic`

Returns the converted value or nil if parsing fails.

---

## ParseCsv

```lua
ParseCsv(csv, [hasHeaders], [columnDelimiter], [rowDelimiter])
```

Parses CSV data string and returns it as an object. URLs are also supported.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `csv` | `string` | Yes | The CSV data string or URL to download CSV data from. |
| `hasHeaders` | `boolean` | No | Whether or not data contains headers (default: false) |
| `columnDelimiter` | `string` | No | Delimiter for columns (default: ';') |
| `rowDelimiter` | `string` | No | Delimiter for rows (default: '\n') |

**Returns:** `dynamic`

Object containing the parsed information.

---

## ParseJson

```lua
ParseJson(json)
```

Parses JSON data string and returns it as an object. URLs are also supported.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `json` | `string` | Yes | The JSON data string or URL to download JSON data from. |

**Returns:** `dynamic`

Object containing the parsed information.

---

## StringContains

```lua
StringContains(value, searchValue, [ignoreCase])
```

Returns a value indicating whether a specified substring occurs within this string.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `value` | `string` | Yes | The main string. |
| `searchValue` | `string` | Yes | The substring to search. |
| `ignoreCase` | `boolean` | No | Ignoring the case of the scripts being compared. |

**Returns:** `boolean`

Returns true if the specified substring occurs within this string.

---

## StringExplode

```lua
StringExplode(value, delimiter)
```

**Aliases:** `StringSplit`

Splits a string into substrings that are based on the characters in an array.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `value` | `string` | Yes | The main string. |
| `delimiter` | `string` | Yes | The delimiter value. |

**Returns:** `unknown`

Returns an array of strings.

---

## StringFromQuery

```lua
StringFromQuery(value)
```

Split a string on '&' and '='.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `value` | `string` | Yes | The main string. |

**Returns:** `unknown`

Returns an key-value based table.

---

## StringIndexOf

```lua
StringIndexOf(value, searchValue, [ignoreCase])
```

Gets the zero-based index of the first occurrence of the specified string in the current.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `value` | `string` | Yes | The main string. |
| `searchValue` | `string` | Yes | The search value. |
| `ignoreCase` | `boolean` | No | Ignoring the case of the scripts being compared. |

**Returns:** `number`

Returns a zero-based index of the first occurrence of the specified string.

---

## StringJoin

```lua
StringJoin(value1, value2, [separator])
```

Concatenates 2 string with an optional separator.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `value1` | `dynamic` | Yes | The first part of the string. |
| `value2` | `dynamic` | Yes | The second part of the string. |
| `separator` | `string` | No | The string to use as a separator. |

**Returns:** `string`

Returns the concatenated string.

---

## SubString

```lua
SubString(value, start, length)
```

Retrieves a substring from this instance. The substring starts at a specified character position and has a specified length.

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `value` | `string` | Yes | The main string. |
| `start` | `number` | Yes | Start index, zero based. |
| `length` | `number` | Yes | The length of the substring. |

**Returns:** `string`

Substring of the main value.

---


*Auto-generated from HaasScript source code.*
