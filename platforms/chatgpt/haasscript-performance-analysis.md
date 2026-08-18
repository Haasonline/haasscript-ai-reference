# HaasScript Performance Analysis --- HaasOnline Cloud v2

## Purpose

Analyze backtests, Labs, and live bot performance and turn the evidence
into a clear verdict, primary constraint, diagnosis, and next action.

Use the shared `HaasOnline Cloud Core Rules` for Cloud mechanics.

## 1. Required output

Every analysis must contain: 1. Headline verdict: PASS / CONDITIONAL /
FAIL 2. Primary constraint 3. Key evidence/metrics 4. Root cause 5.
Concrete next actions

Never declare production readiness from one run or one Lab winner.

## 2. Strategy classification

Identify the strategy class first when possible: trend, momentum, mean
reversion, grid, DCA, market making, scalping, arbitrage, long-only
spot, directional futures, hybrid.

Interpret ROI, drawdown, turnover, trade count, fees, and benchmark
behavior according to that class. Generic numeric thresholds are
heuristics, not universal laws.

## 3. Evidence collection

For a standalone backtest: - `get_backtest_info` for headline metrics -
`get_backtest_runtime` for actual runtime/report/input state -
`get_backtest_positions` for trade-level evidence - `get_backtest_logs`
for execution problems

For Lab results: - `get_lab_results` - discard failed/partial entries
before ranking - filter zero-trade configurations before calling them
winners - drill promising/diagnostic combinations with both `lab_id` and
`BID`

For live bots: - `get_bot_runtime` - `get_bot_positions` -
`get_bot_open_orders` - `get_bot_report` / profits where appropriate

## 4. Standalone trade-count rule

On the standalone runtime path, `FinishedPositions` is always empty by
design. Use `Reports.P.C` for closed-count and `get_backtest_positions`
for the actual positions. Do not interpret the empty field as zero
trades.

## 5. Benchmark and alpha

Compare strategy ROI with buy-and-hold over the exact same period:
`alpha = strategy ROI - buy-and-hold ROI`.

Obtain the benchmark from actual market data, not memory. For simple
spot buy-and-hold:
`hold ROI = (ending close / starting close - 1) * 100`.

For futures, leveraged, short, or otherwise non-equivalent strategies,
state that simple spot buy-and-hold is only a reference benchmark and
add a more appropriate comparison when possible.

## 6. Fee and edge diagnosis

Compute: `fee drag = total fees / gross profit`, when gross profit \> 0.

Also compare: `average gross profit per trade` against the estimated
round-trip transaction cost.

Interpretation: - high fee drag + many trades → reduce turnover,
increase edge per trade, or reduce fees - low fee drag + poor ROI →
investigate signal quality, regime mismatch, or risk - never recommend
larger size as a cure for fee drag; scaling notional does not change the
fee-to-gross ratio.

## 7. Profit factor and risk

Profit factor: `gross profit / gross loss`.

Report: - ROI - alpha - max drawdown - fees / fee drag - profit factor -
Sharpe where meaningful - trade count - win rate - average edge per
trade - exposure/position behavior - sample size.

Do not treat a small sample as proof. Explicitly flag low-trade results
as hypotheses.

## 8. Lab analysis

Before ranking: 1. remove failed/partial executions 2. remove zero-trade
configurations when assessing trading performance 3. inspect parameter
neighborhoods 4. compare worst-regime performance 5. validate top
candidates on an independent/longer horizon.

A high ROI with extreme drawdown or unstable neighboring parameters is
weaker evidence than a slightly lower ROI with a stable neighborhood.

If a directional strategy trades most heavily during the regime opposing
its direction, investigate structural mismatch before sweeping more
parameters.

## 9. Regime validation

Use deliberately chosen market regimes appropriate to the strategy: -
adverse/hostile regime - favorable regime - sideways/chop regime -
longer validation horizon within the server/license limit.

Do not hardcode one set of calendar months forever. Select windows from
the target market and validation period.

Use gates appropriate to strategy class. A generic example: - hostile
regime: avoid catastrophic drawdown/position runaway - favorable regime:
participate appropriately - sideways regime: control churn/fee drag -
longer horizon: positive risk-adjusted performance and useful alpha.

## 10. Diagnosis map

### Zero trades

Check logs → runtime/input values → market data → account/market
compatibility → sizing → interval → strategy conditions.

### Positive ROI but negative alpha

The strategy made money but underperformed holding. Identify whether the
shortfall occurs mainly in bullish periods or universally.

### Negative ROI with heavy turnover

Measure fee drag and edge-per-trade first.

### High drawdown

Inspect position-level losses, concentration, leverage, stop behavior,
and regime exposure.

### Lab vs standalone disagreement

Compare: - actual compiled/runtime Inputs - account/market - trade
amount - interval - leverage/modes - exact date range - script
version/compile state.

## 11. Robustness

Prefer: - stable parameter neighborhoods - consistent behavior across
regimes - adequate trade count - sensible drawdown - positive or
defensible alpha - fee-adjusted edge.

Avoid selecting a single isolated ROI peak.

## 12. Live vs backtest

Separate expected differences from red flags: Expected: slippage,
latency, fee differences, fill differences. Investigate: large
trade-frequency changes, recurring rejected orders, materially worse
realized economics, or parameter/configuration mismatch.

MCP live-bot control is read-only. Recommendations requiring changes
must be applied through HTS.

## 13. Reporting template

``` text
## Performance Analysis
Strategy:
Class:
Period:
Market:
Account:

### Verdict
PASS / CONDITIONAL / FAIL

### Key Metrics
ROI:
Buy-and-hold:
Alpha:
Max drawdown:
Fees / fee drag:
Profit factor:
Sharpe:
Trade count:
Win rate:
Avg edge/trade:

### Primary Constraint
...

### Root Cause
...

### Regime / Robustness
...

### Recommended Next Actions
1. ...
2. ...
3. ...

### Evidence / Parameters Used
...
```
