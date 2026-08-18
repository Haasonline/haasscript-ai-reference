# HaasScript Export Instrumentation --- HaasOnline Cloud v2

## Purpose

Capture the values used at real entry/exit decisions, retrieve them
through Cloud MCP, join them to realized trades, and use the evidence to
form testable optimization hypotheses.

Use the shared `HaasOnline Cloud Core Rules` for Cloud mechanics.

## 1. Core principle

Capture at the **decision edge**, immediately before the strategy
commits to an action. Do not tie capture to managed-position state when
the purpose is to study the decision itself.

Record the full feature vector used by the decision: - indicators -
thresholds - derived values - trend/state flags - price - direction -
any relevant configuration.

Prefer one row per real decision, not one row per tick.

## 2. Choose the capture pattern

### A. Decision events

Default. Best for optimization and trade-level analysis. Log ENTRY/EXIT
rows when the decision transitions false → true.

### B. Per-tick/bar series

Use only when indicator behavior between decisions is required. Round
numeric values and gate writes to a useful cadence.

### C. Scalar metrics

Use `CustomReport()` for end-of-run metrics that do not require
row-level retrieval.

## 3. Instrumentation must be additive

Do not alter strategy logic while instrumenting. Compile the
instrumented source before saving it. Reset instrumentation state in
`Finalize()` when `Save/Load` flags are used.

Typical pattern:

``` lua
local trigger = condition
local previous = Load("previousTrigger", 0)

if trigger and previous == 0 then
    WriteExportData("Triggers", Time(), "ENTRY", Round(price, 2), Round(feature, 2))
    -- existing trade action remains unchanged
end

Save("previousTrigger", trigger and 1 or 0)

Finalize(function()
    Save("previousTrigger", 0)
end)
```

## 4. Export design

-   Keep headers stable and descriptive.
-   The number of values passed to `WriteExportData` must match
    `InitExportData`.
-   Round floats when high precision is unnecessary.
-   Avoid per-tick exports unless explicitly required.
-   Do not assume the chart `interval` changes script execution
    frequency; budget export rows according to actual execution cadence.

Decision-event logging is normally tiny. Per-tick exports can become
large and should be paginated.

## 5. Entry/exit semantics

Logged price is the decision-time price, not necessarily the eventual
fill. Join decisions to ledger trades by timestamp with a reasonable
interval-aware tolerance, not exact price equality.

Engine-driven exits such as stops/trailing exits may not produce a
script exit row. Preserve them as ledger outcomes with an
unmatched/script-exit source distinction.

For unmanaged trading, verify the exact order command signature with
`search_haasscript_docs` before implementing it.

## 6. Cloud pre-flight

Before an instrumented backtest: 1. compile the instrumented script 2.
resolve account and market 3. verify market data 4. build complete
validated settings 5. use a fresh backtest UUID 6. execute and retain
the returned `service_id` 7. poll to terminal 8. inspect logs 9. verify
positions/runtime 10. retrieve export rows.

`orderTemplate` is optional in the current `execute_backtest` schema and
defaults to `500` if absent/invalid. Prefer an explicit validated value
in hand-built settings.

## 7. Retrieve exports

Use:
`get_backtest_export_data(backtest_id, name, format, next_page_id, page_length)`

-   The export name must exactly match the `InitExportData` name.
-   Start at offset 0.
-   Follow returned `NP`.
-   Do not assume the whole file fits in one response.
-   Lower `page_length` for wide rows if transport limits are reached.
-   For Lab combos, pass both `lab_id` and the combo `BID`.

`get_backtest_runtime.Files` exposes export names/headers, not the data
rows. Use `get_backtest_export_data` for rows.

## 8. Join to outcomes

Join decision rows to `get_backtest_positions` using: - timestamp -
direction - interval-aware tolerance.

Produce one row per realized trade containing: - entry features - exit
features when available - realized P&L - ROI - fees - hold time -
win/loss - match/source flags.

If paginating positions, concatenate all `I` pages before analysis.

## 9. Analyze the dataset

Split analysis by direction.

For winners vs losers: - compare feature distributions, not just means -
bucket continuous indicators when useful - inspect sample size first -
look for consistent separation rather than a single outlier - form one
hypothesis per iteration.

A small winner/loser sample is a hypothesis, not evidence of a reliable
threshold.

## 10. Turn evidence into optimization

Correct workflow: 1. identify a feature separation 2. state one
hypothesis 3. expose the relevant threshold as an `Input` 4. sweep it in
Labs 5. filter failed/partial/zero-trade results 6. compare robust
parameter neighborhoods 7. validate across hostile/favorable/sideways
regimes 8. confirm on a longer standalone window 9. compare against the
appropriate benchmark.

Do not hand-pick the exact observed winner/loser mean as the new
threshold. That is prone to overfitting.

## 11. Common failure modes

-   Empty export: wrong export name, zero decisions, or early execution
    failure. Check logs.
-   Export stops mid-run: inspect logs at the stop timestamp.
-   Positions empty but triggers exist: order execution may have failed;
    inspect logs/runtime.
-   Stale state: reset `Save/Load` flags in `Finalize()`.
-   Short decisions never fill: verify the account supports short
    exposure.
-   Huge export: switch from per-tick to decision-event capture or
    reduce columns/precision.
-   Lab/standalone mismatch: compare actual runtime Inputs/settings
    before interpreting the feature distributions.

## 12. Minimal managed example

``` lua
InitExportData("Triggers",
    "Timestamp", "Event", "Price", "RSI", "Trend")

local rsiArr = RSI(ClosePrices(), 14)
local rsi = rsiArr[1]
local trend = EMA(ClosePrices(), 12)[1] > EMA(ClosePrices(), 26)[1]

local enter = rsi < 30 and trend
local prev = Load("prevEnter", 0)

if enter and prev == 0 and GetPositionDirection() == NoPosition
   and not IsAnyOrderOpen() then
    WriteExportData("Triggers",
        Time(), "ENTRY_LONG", Round(ClosePrices()[1], 2),
        Round(rsi, 2), trend and 1 or 0)
    DoLong("instrumented entry")
end

Save("prevEnter", enter and 1 or 0)

Finalize(function()
    Save("prevEnter", 0)
end)
```

Verify every command/signature against `search_haasscript_docs` before
adapting an example.

## 13. Output

For instrumentation work report: - what was instrumented - decision
columns captured - compile/validation result - backtest verification -
export row count - join coverage - winner/loser evidence - hypothesis -
proposed Lab sweep - validation plan.

Never claim the instrumentation or export worked without checking the
compiled script, logs, runtime, and retrieved rows.
