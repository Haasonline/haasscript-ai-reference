---
name: haasscript-export-instrumentation-cloud
description: >
  Instrument HaasScript strategies on HaasOnline Cloud to capture the indicator and
  calculated values that trigger each entry and exit decision, then join those decision
  feature-vectors to each trade's realized outcome so you can tune the script for profit.
  Use whenever the user wants trade-level evidence for optimisation on a Cloud server —
  "what indicator values did my losing trades fire at?", "capture the trigger conditions
  on every entry and exit", "log the values used to enter/exit so I can tune thresholds",
  "why do my entries lose?", "build a dataset of trigger values vs P&L", or any request to
  add InitExportData/WriteExportData logging, retrieve the rows over MCP, and join them to
  the position ledger. Cloud customers have no filesystem access to the trade server, so
  everything here runs through get_backtest_export_data. Includes the verified export
  payload budget, the 31-day lab cap, plan-capped standalone windows, and the
  orderTemplate setting that silently breaks every instrumented backtest without it.
---

# HaasScript Export Instrumentation — HaasOnline Cloud Edition

## Purpose

A backtest report tells you *what* happened (ROI, win rate, fees). It cannot tell you the
**indicator and calculated values that caused each entry and exit** — the actual trigger
conditions. This skill captures those decision-time values per trade and joins them to each
trade's realized P&L, producing the dataset you tune from: *which trigger values separate
winners from losers, so you know which threshold to move.*

The end goal is **optimisation for profit**: instrument → backtest → join trigger values to
outcomes → see where winners and losers differ → adjust thresholds (usually via a Labs
sweep) → re-validate across regimes.

This edition is written for any HaasOnline customer using the **Cloud** MCP connector. It
contains no server-specific IDs, dates, or account references — resolve those at runtime
with `list_scripts`, `list_accounts`, `list_markets`, `list_labs`. Everything here was
verified by live probing against a production HaasOnline Cloud server in July 2026; the
measured numbers are marked as such.

### Who this applies to

Per HaasOnline's published plan comparison, the **MCP Server**, **HaasLabs**, and **Export
Settings & Data** are all *excluded from the Starter tier*. This workflow therefore needs
**Standard or above**. The three cloud-hosted tiers are Starter, Standard and Pro;
TradeServer runs self-hosted or on managed private cloud, so a customer connecting to a
HaasOnline **Cloud** server through MCP is on **Standard or Pro**.

That resolves the two window caps below to single values for this audience: a **31-day lab
window** and a standalone window of **186 days (Standard)** or **372 days (Pro)**.

---

## What changes on Cloud

On a self-hosted TradeServer you can reach into
`…\Cache\Spaces\Backtest\Files\<backtest_id>-<Name>.json.gzip`, copy the base64+gzip blobs
off the host, and decode them locally. **On Cloud you cannot.** There is no filesystem, no
PowerShell retrieval step, and no decode step.

Instead there is exactly one route, and it is better: the **`get_backtest_export_data`**
MCP tool returns your export rows directly as CSV (or JSON), already decoded, for both
standalone backtests and individual lab combos. The entire loop — instrument, run, retrieve,
join, tune — happens over MCP.

The practical consequences:

- **You do not need `retrieve_exports.ps1` or `decode_export.py`.** Delete those steps from
  any self-hosted workflow you are porting. `format="csv"` gives you a file you can write
  straight to disk and read with `csv.DictReader`.
- **The server does not truncate.** A 31-day per-minute export came back **complete at 3.45
  MiB / 44,641 rows** in a single call. The tool's "~1MB transport cap" note describes what
  *some MCP clients* can ingest, not a server-side limit.
- **Your MCP client is the real constraint.** An oversized response gets spilled to a file
  or truncated by the client, not by HaasOnline. Budget your rows (see below) so the
  response stays comfortable, and when it does spill, read the spill file with a script
  rather than re-calling.
- **Window caps shape the run, not the retrieval.** Labs are capped at 31 days on every
  Cloud tier and standalone backtests at 186 or 372 days depending on plan, so long-horizon
  instrumentation means several chained windows.

---

## Cloud limits (verified by live probing, July 2026)

| Limit | Value | Notes |
|---|---|---|
| **Lab execution window** | **exactly 31 days** (2,678,400 s) on every Cloud tier | This is the plan comparison's *Max Test Depth* row: 1 Month on both Standard and Pro. Verified on Pro: 31d accepted, 31d + 1h rejected with `"Backtest period cannot exceed 1 month."` (TradeServer gets 12 months, but TradeServer is self-hosted or managed, not Cloud.) |
| **Standalone backtest window** | **186 days (Standard) / 372 days (Pro)** | The plan comparison's *Backtesting* row: up to 6 and 12 months respectively, where a "month" is implemented as **31 days**. Verified on Pro: 372d (= 12 × 31) accepted, 373d rejected with `"License limit"`. The Standard figure is the same arithmetic, unverified — probe it. |
| **Window vs lookback** | Limit is on **window length only** | A 31-day window five years back is fine. Confirm data exists first with `check_market_data` (no auth required). |
| **Concurrent backtests, same script** | **Not allowed** | `"Cannot execute multiple backtests of the same script at once"`. Serialise, or clone the script under a second name to parallelise. |
| **Concurrent backtests, different scripts** | Allowed | Each gets its own `service_id`. Observed running side by side without interference. |
| **Export payload from the server** | **No cap observed** | 3.45 MiB / 44,641 rows returned complete and well-formed in one `get_backtest_export_data` call. |
| **Export payload into the MCP client** | ~1 MiB comfortable | Beyond that, expect client-side spill or truncation. This is the number to budget against. |
| **Lab export drill-in** | Full rows, uncapped | `lab_id` + the result entry's `BID` returned all 44,641 rows (1.93 MiB) for a single combo. Unlike `get_backtest_positions`, which clamps `page_length` to 1000 on the lab path. |
| **`get_lab_results` page size** | Keep `page_length` ≤ 45 | ~1.1 KB per entry; 81 entries ≈ 95 KB already strains most clients. |
| **`list_accounts` / `list_scripts`** | 100–500 KB on mature accounts | Measured 473 KB for `list_accounts`. Filter and summarise; never dump raw. |
| **New-lab `tradeAmount` default** | **100.0**, silently | `create_lab` ignores your intended size until you set it via `update_lab`. Results are incomparable with standalone runs until fixed. |
| **Live bots** | Read-only over MCP | Observe state, positions, orders, logs. Create/start/stop happens in the HTS web interface. |
| **Rejected window calls** | Free | They error instantly and start nothing, so probing your own tier's boundary costs nothing. |

**Throughput reference:** 4 lab combos over a 31-day 1h window finished in ~3 minutes; 3
concurrent labs × 81 combos of a 1-month 1h backtest finished in ~13 minutes total.

### Published plan limits

From HaasOnline's published plan comparison
([haasonline.com/pricing](https://haasonline.com/pricing), July 2026). The rows that
constrain instrumentation work:

| | Starter | Standard | Pro | TradeServer |
|---|---|---|---|---|
| Infrastructure | Cloud | Cloud | Cloud | Self-hosted or Managed |
| **MCP Server** | **not included** | included | included | included |
| **HaasLabs** | **not included** | included | included | included |
| **Export Settings & Data** | **not included** | included | included | included |
| Backtesting (standalone window) | up to 1 month | up to 6 months | up to 12 months | up to 36 months |
| Max Test Depth (lab window) | — | 1 Month | 1 Month | 12 Months |
| Trade bots | 3 | 8 | 20 | 100+ |
| Max orders per bot | 15 | 25 | 50 | 150 |

Two things follow. **Starter cannot run this workflow at all** — no MCP connector, no Labs,
no data export. And a Cloud MCP customer is on Standard or Pro, both of which have a
**1-month lab cap**, so every sweep in this document is a 31-day sweep. Only TradeServer
raises lab depth to 12 months, and TradeServer is not Cloud-hosted.

Treat these as the published intent and the probe as ground truth: a "month" resolves to 31
days in the engine, and a rejected window call errors instantly without starting anything,
so confirming your own boundary costs nothing.

---

## The export payload budget (measured)

This is the number that decides your instrumentation design on Cloud. Two facts drive it:

1. **Per-tick logging fires every minute regardless of `interval`.** A `WriteExportData`
   at the top level of a script running at `interval=60` produced **44,641 rows over 31
   days** — minutes in the period, not hourly bars. Budget in minutes, always.
2. **HaasScript writes full float precision unless you round.** A raw `RSI(...)` value
   serialises as `45.4484050018493`. Wrapping it in `Round(rsi, 2)` writes `45.45`.

Measured on a 6-column row (`Timestamp, Price, RSI, EMAfast, EMAslow, PosDir`):

| Row style | Bytes/row | 7 days | 31 days | 186 days | 372 days |
|---|---|---|---|---|---|
| Full precision | **80.9** | 0.78 MiB | 3.44 MiB | 20.7 MiB | 41.3 MiB |
| `Round(x, 2)` on every float | **45.3** | 0.44 MiB | **1.93 MiB** | 11.6 MiB | 23.2 MiB |

**Rounding to 2 decimals cut the payload by 44%** — the single cheapest optimisation
available. It costs nothing analytically: no threshold worth tuning lives in the 14th
decimal place.

How much per-minute history fits in one comfortable response:

| Budget | Full precision | Rounded |
|---|---|---|
| 1 MiB | 9.0 days | **16.1 days** |
| 2 MiB | 18.0 days | 32.1 days |
| 4 MiB | 36.0 days | 64.3 days |

And the number that matters most:

> **Decision-event logging is effectively free.** Measured at **64.2 bytes per row** for an
> 8-column event row. A strategy taking 200 round trips logs 400 rows ≈ **25 KiB**. You can
> instrument every entry and exit decision across a full 372-day Pro-tier standalone
> backtest and never come close to a payload problem.

**The rule this produces:** always run decision-event capture (Pattern A/B). Add per-tick
`FullSeries` capture only when you specifically need indicator behaviour between trades, and
when you do, round every value and keep the window to roughly a fortnight — or gate the
write to interval-aligned bars.

---

## The core principle: capture at the decision, not the order

The trigger values live in your **script's decision logic** — everything computed before the
`if` that fires an order. Capture them *right there*, at the decision edge, regardless of
how the order is then executed. This is what makes capture work identically for **managed**
(`DoLong`/`DoShort`/`DoExitPosition`) and **unmanaged** (`PlaceBuyOrder`/`PlaceGoLongOrder`/…)
trading: both run through the same script-side decision. Don't gate capture on managed
position state — that locks you to one mode and misses engine-driven exits.

Two rules that make the captured data useful:

- **Log the full feature-vector**, not one number. Record *every* value the decision uses —
  all indicators, the thresholds in play, any derived or intermediate values. You cannot
  tune on a feature you did not log, and re-running to add one column costs a whole backtest.
- **One row per real decision.** Edge-gate the write (log when the condition transitions
  false→true, tracking the previous state in `Save`/`Load`) so a signal that stays true for
  many bars logs once, not every minute.

---

## Workflow

1. **Instrument.** Add a decision-edge capture block (Pattern A or B below) and widen its
   columns to your strategy's full feature-vector. Wrap every float in `Round(x, 2)`. Keep
   strategy logic intact — instrumentation is purely additive. Verify with `compile_script`
   before saving; it needs no authentication and costs nothing.
2. **Pre-flight.** `health_check`, then resolve `account_id` with `list_accounts`, the market
   tag with `list_markets`, and confirm data for the window with `check_market_data`. This
   is blocking, not advisory.
3. **Build the settings block correctly.** This is where instrumented Cloud backtests most
   often die silently — see *The `orderTemplate` trap* below. Copy a known-good block from
   an existing lab via `get_lab_details` rather than hand-assembling one.
4. **Run.** `execute_backtest`, store the exact `service_id` string it returns, poll
   `get_backtest_status` to `S=3`. Run short-side strategies on a margin/futures account —
   on spot, `DoShort` is a no-op and short decisions log but never fill.
5. **Verify the run before trusting the data.** Check `get_backtest_logs` for execution
   errors and confirm the export row count matches the trade count you expect. A failed run
   still reports `S=3` at 100% (see gotchas).
6. **Retrieve.** `get_backtest_export_data(backtest_id, name, format="csv")` — add `lab_id`
   for a lab combo. Save the returned string verbatim to a `.csv` file.
7. **Join to outcome (the key step).** Save `get_backtest_positions` output to JSON and run
   the join script below. It emits one row per trade — *entry feature-vector + realized P&L
   + win/ROI/hold time*, plus an exit feature-vector when the exit was script-driven — and
   prints a winners-vs-losers breakdown **by direction**.
8. **Tune.** Turn the winners-vs-losers deltas into one threshold hypothesis, encode it as a
   Labs sweep across deliberately chosen regime months, and validate the survivor standalone
   over the longest window your plan allows (186 days on Standard, 372 on Pro).

---

## Capture patterns

### Pattern A — Decision-edge capture, MANAGED (default)

Compiles clean on Cloud. Adapt the indicators and conditions to your strategy; keep the
edge-gate and the `Finalize` reset.

```lua
-- Inputs (1/0 for booleans so Labs can sweep them)
local rsiLength    = Input("RSI Length", 14)
local rsiBuyLevel  = Input("RSI Buy Level", 30)
local rsiExitLevel = Input("RSI Exit Level", 55)
local emaFastLen   = Input("EMA Fast", 12)
local emaSlowLen   = Input("EMA Slow", 26)

-- Decision feature-vector: compute EVERYTHING the decision uses
local closes  = ClosePrices()
local price   = closes[1]
local rsiArr  = RSI(closes, rsiLength);  local rsi  = rsiArr[1]
local emaFArr = EMA(closes, emaFastLen); local emaF = emaFArr[1]
local emaSArr = EMA(closes, emaSlowLen); local emaS = emaSArr[1]
local trendUp = emaF > emaS
local ts      = Time()
local posDir  = GetPositionDirection()

InitExportData("Triggers", "Timestamp", "Event", "Price",
               "RSI", "EMAfast", "EMAslow", "TrendUp", "PosAmount")

-- Entry decision (edge-gated, only when flat)
local enterLong = rsi < rsiBuyLevel and trendUp
local prevEnter = Load("prevEnter", 0)
if enterLong and prevEnter == 0 and posDir == NoPosition and not IsAnyOrderOpen() then
    WriteExportData("Triggers", ts, "ENTRY_LONG", Round(price, 2), Round(rsi, 2),
                    Round(emaF, 2), Round(emaS, 2), trendUp and 1 or 0, GetPositionAmount())
    DoLong("entry: rsi<buy & trend up")
end
Save("prevEnter", enterLong and 1 or 0)

-- Script exit decision (edge-gated)
local exitLong = posDir == PositionLong and rsi >= rsiExitLevel
local prevExit = Load("prevExit", 0)
if exitLong and prevExit == 0 and not IsAnyOrderOpen() then
    WriteExportData("Triggers", ts, "EXIT_LONG", Round(price, 2), Round(rsi, 2),
                    Round(emaF, 2), Round(emaS, 2), trendUp and 1 or 0, GetPositionAmount())
    DoExitPosition("exit: rsi>=neutral")
end
Save("prevExit", exitLong and 1 or 0)

-- Save/Load persists across runs — reset so each backtest starts clean
Finalize(function()
    Save("prevEnter", 0); Save("prevExit", 0)
end)
```

Mirror the blocks for the short side (`ENTRY_SHORT` / `EXIT_SHORT`, `DoShort` /
`DoExitPosition`, gated on `PositionShort`) and run those on a margin/futures account.

Two things to expect in the output. `PosAmount` reads **0 on entry rows** — the fill is not
booked within the same tick, which is normal and not a bug. And the logged price is the
**decision-bar close**, not the fill; reconcile on timestamp, never on exact price equality.

**Engine-driven exits** (stop-loss, take-profit, trailing) never pass through this block —
the position closes without your exit branch firing. Don't try to attribute them in-script;
the join surfaces them as a ledger trade with no matching exit-decision row. If you want
their context, log the configured SL/TP levels as extra columns on the entry row.

### Pattern B — Decision-edge capture, UNMANAGED

Same idea for unmanaged order flow: capture at the `Place*` call and track your own position
flag. HaasScript will warn *"No order handling command detected"* — expected. For trigger
capture you don't need fill polling (the join supplies outcomes from the ledger); for
production add order-handling checks.

```lua
local rsiLength    = Input("RSI Length", 14)
local rsiBuyLevel  = Input("RSI Buy Level", 30)
local rsiSellLevel = Input("RSI Sell Level", 70)
local orderAmt     = Input("Order Amount", 0.01)

local closes = ClosePrices()
local price  = closes[1]
local rsiArr = RSI(closes, rsiLength); local rsi = rsiArr[1]
local ts     = Time()
local myPos  = Load("myPos", 0)   -- 0 flat, 1 long (track manually when unmanaged)

InitExportData("Triggers", "Timestamp", "Event", "Price", "RSI", "Amount")

local enterLong = rsi < rsiBuyLevel
local prevEnter = Load("prevEnter", 0)
if enterLong and prevEnter == 0 and myPos == 0 then
    WriteExportData("Triggers", ts, "ENTRY_LONG", Round(price, 2), Round(rsi, 2), orderAmt)
    PlaceBuyOrder(orderAmt, price)   -- spot: (amount, price)
    Save("myPos", 1)
end
Save("prevEnter", enterLong and 1 or 0)

local exitLong = myPos == 1 and rsi > rsiSellLevel
local prevExit = Load("prevExit", 0)
if exitLong and prevExit == 0 then
    WriteExportData("Triggers", ts, "EXIT_LONG", Round(price, 2), Round(rsi, 2), orderAmt)
    PlaceSellOrder(orderAmt, price)
    Save("myPos", 0)
end
Save("prevExit", exitLong and 1 or 0)

Finalize(function()
    Save("prevEnter", 0); Save("prevExit", 0); Save("myPos", 0)
end)
```

Order-command signatures differ by market type: spot is `PlaceBuyOrder(amount, price)` /
`PlaceSellOrder(amount, price)`; futures is `PlaceGoLongOrder(price, amount)` /
`PlaceGoShortOrder(price, amount)` — price and amount are reversed. Confirm with
`search_haasscript_docs` before writing either.

### Pattern C — Per-tick series, Cloud-lean

Only when you need indicator behaviour *between* trades. Round everything, emit direction as
a small integer rather than the enum's string form, and keep the window near a fortnight.

```lua
InitExportData("Series", "Timestamp", "Price", "RSI", "EMAfast", "EMAslow", "PosDirN")

local posN = 0
if posDir == PositionLong then posN = 1 elseif posDir == PositionShort then posN = -1 end

WriteExportData("Series", ts, Round(price, 2), Round(rsi, 2),
                Round(emaF, 2), Round(emaS, 2), posN)
```

To cut per-minute rows down to interval-aligned bars — a 60× reduction at `interval=60`:

```lua
if Time() % (60 * 60) == 0 then WriteExportData("Series", ...) end
```

### Pattern D — Scalar metrics, no retrieval

For values you only need as one end-of-run number, skip export files entirely.
`CustomReport()` surfaces in the backtest report and in `get_bot_runtime` for live bots.

```lua
Finalize(function()
    CustomReport("Fee Drag Ratio", Load("totalFees", 0) / Max(Load("grossProfit", 1), 0.001))
end)
```

### Pre-compile checklist

- All `Input()` calls at top level; booleans use `1`/`0` so Labs can sweep them.
- Every `Save` key has a `Load` with a default, and every edge/state flag resets in `Finalize`.
- No `math.*` — use HaasScript builtins (`Round`, `Max`, `Min`, `Abs`). `Round(input, digits)`.
- Array access starts at index **1**.
- Value count in `WriteExportData` must equal the header count in `InitExportData`.
- `InitExportData` is called every tick by design — it is idempotent registration and does
  not truncate prior rows.
- Short-side capture needs a margin/futures account to actually fill.

---

## Running an instrumented backtest

### The `orderTemplate` trap — read this before your first run

An `execute_backtest` settings block that omits `orderTemplate` **compiles, launches,
completes, and reports `S=3` at 100% — while every single trade action fails.** The symptoms
are easy to misread:

- `get_backtest_positions` returns an empty list.
- The export file contains rows right up to the first decision, then **stops dead**.
- Only `get_backtest_logs` reveals the cause:
  `ERROR: DoLong(): Execution error occurred.` / `ERROR: Argument out of range exception.`

This reproduces on a bare `DoLong()` with no arguments, on both simulated and real accounts,
at any trade amount. It is the settings block, not your script.

Use the full shape:

```json
{
  "botId": "", "botName": "",
  "accountId": "<from list_accounts>",
  "marketTag": "<from list_markets>",
  "positionMode": 0, "marginMode": 0, "leverage": 0,
  "tradeAmount": 0.01,
  "interval": 60,
  "chartStyle": 300,
  "orderTemplate": 500,
  "scriptParameters": {}
}
```

The reliable way to get this right on any server: call `get_lab_details` on a lab that has
completed successfully and copy its `ST` block, swapping only the account and market. Labs
populate `orderTemplate` themselves, which is why lab runs of the same script succeed while
hand-rolled standalone runs fail.

Also note `tradeAmount` is **base-denominated** — `0.01` on a BTC market means 0.01 BTC, not
$0.01. And `create_lab` silently defaults it to `100.0`, so always override it in
`update_lab` or your lab and standalone results will not be comparable.

### Always verify the run before trusting the data

Because a failed run looks identical to a successful one at the status level, make these
three checks a habit:

1. `get_backtest_logs(backtest_id, next_page_id=0, page_length=20)` — any line containing
   `ERROR:` invalidates everything after its timestamp.
2. Row count sanity — an event file should hold roughly `trades × 2` rows. Thousands means a
   gating bug; a handful when you expected dozens means the run died early.
3. `get_backtest_positions` returns a non-empty ledger.

A **spot-short flood** is the classic gating bug: on a spot account `DoShort` is a no-op, so
a `posDir == NoPosition` gate never clears and the short branch re-fires every minute the
signal holds. Edge-gating (Pattern A) makes no-ops simply never log.

---

## Retrieving the rows

### Standalone backtest

```
get_backtest_export_data(
    backtest_id = "<the id you generated>",
    name        = "Triggers",        # the InitExportData name, case-sensitive
    format      = "csv"              # or "json" for an array of row objects
)
```

The result is a plain CSV string — header line plus data lines. Save it verbatim to
`triggers.csv`.

**An empty result (headers only, or nothing) means one of three things**, in order of
likelihood: the `name` doesn't match your `InitExportData` name exactly; the strategy made
zero decisions in that window; or the run errored before the first write. Check the logs
before assuming your instrumentation is wrong.

### Lab combo

Pass `lab_id` alongside the result entry's `BID` — no re-run as a standalone is needed:

```
results = get_lab_results(lab_id, next_page_id=0, page_length=45)
bid     = results.I[0].BID
get_backtest_export_data(backtest_id=bid, lab_id=lab_id, name="Triggers", format="csv")
```

Verified: this returns the full per-combo export, with values that genuinely differ per
parameter set. There is no clamping on this path — a 44,641-row file came back intact.

Because each combo has its own export file, a sweep gives you *N* trigger datasets. That is
the most powerful thing this skill enables on Cloud: you can compare the actual decision
distributions of the winning and losing parameter sets, not just their ROI. Pull exports for
the top few and the bottom few combos and diff their entry feature distributions.

### Payload discipline

Cap page sizes on everything you call alongside the export: `get_lab_results` at
`page_length ≤ 45`, positions and logs at `limit ≤ 20–50`, and paginate. When a response
does spill to a file in your client, process the spill file with a script rather than
re-issuing the call.

---

## The join — turning captures into a tuning dataset

Save `get_backtest_positions` output verbatim to `positions.json` (concatenate the `I`
arrays if you paginated), then run this against it and your `triggers.csv`.

```python
#!/usr/bin/env python3
"""
join_outcomes_cloud.py -- join HaasScript decision-trigger rows to realized trade outcomes.

Inputs (both produced entirely over the Cloud MCP -- no HTS host access needed):
  triggers.csv   the string returned by get_backtest_export_data(..., format="csv"),
                 saved verbatim to a file.
  positions.json the full JSON response of get_backtest_positions(...), saved verbatim.
                 Concatenate the "I" arrays of every page if you paginated.

Output:
  trades_dataset.csv  one row per ledger trade: entry feature-vector + realized outcome
                      (+ exit feature-vector when the exit was script-driven)
  stdout              winners-vs-losers feature breakdown, split by direction

Usage:
  python3 join_outcomes_cloud.py triggers.csv positions.json [--tolerance-bars 2] \
                                [--interval 60] [--out trades_dataset.csv]
"""
import argparse
import csv
import json
import sys

ENTRY_PREFIX = "ENTRY"
EXIT_PREFIX = "EXIT"


def load_triggers(path):
    with open(path, newline="") as fh:
        rows = list(csv.DictReader(fh))
    if not rows:
        sys.exit(f"{path}: no data rows. Empty export = wrong file name in "
                 f"get_backtest_export_data, zero decisions, or a run that errored early "
                 f"(check get_backtest_logs).")
    for r in rows:
        r["_ts"] = int(float(r["Timestamp"]))
    return rows


def load_positions(path):
    with open(path) as fh:
        blob = json.load(fh)
    if isinstance(blob, dict) and "result" in blob:
        blob = blob["result"]
    if isinstance(blob, dict) and "I" in blob:
        blob = blob["I"]
    if not isinstance(blob, list):
        sys.exit(f"{path}: expected the get_backtest_positions response "
                 f"(or its result.I array).")
    return blob


def feature_columns(rows):
    skip = {"Timestamp", "Event", "_ts"}
    return [c for c in rows[0] if c not in skip]


def direction_of(pos):
    d = pos.get("d", 0)
    return "SHORT" if d == 1 else "LONG"


def pick(rows, want_prefix, target_ts, tol, direction):
    """Closest decision row of the right kind within +/- tol seconds of target_ts."""
    best, best_gap = None, None
    for r in rows:
        ev = (r.get("Event") or "").upper()
        if not ev.startswith(want_prefix):
            continue
        if direction and direction in ("LONG", "SHORT") and direction not in ev:
            continue
        gap = abs(r["_ts"] - target_ts)
        if gap <= tol and (best_gap is None or gap < best_gap):
            best, best_gap = r, gap
    return best


def main():
    ap = argparse.ArgumentParser()
    ap.add_argument("triggers")
    ap.add_argument("positions")
    ap.add_argument("--tolerance-bars", type=int, default=2)
    ap.add_argument("--interval", type=int, default=60,
                    help="candle interval in MINUTES (match the backtest setting)")
    ap.add_argument("--out", default="trades_dataset.csv")
    args = ap.parse_args()

    trig = load_triggers(args.triggers)
    positions = load_positions(args.positions)
    feats = feature_columns(trig)
    tol = args.tolerance_bars * args.interval * 60

    out_rows, unmatched_entry, engine_exit = [], 0, 0
    for pos in positions:
        ot, ct = int(pos.get("ot", 0)), int(pos.get("ct", 0))
        direction = direction_of(pos)
        pnl = float(pos.get("rp", 0.0))
        roi = float(pos.get("roi", 0.0))
        fees = float(pos.get("fe", 0.0))

        e_row = pick(trig, ENTRY_PREFIX, ot, tol, direction)
        x_row = pick(trig, EXIT_PREFIX, ct, tol, direction)
        if e_row is None:
            unmatched_entry += 1
        if x_row is None:
            engine_exit += 1

        rec = {
            "entry_time": ot, "exit_time": ct,
            "hold_minutes": round((ct - ot) / 60.0, 1) if ct and ot else "",
            "direction": direction,
            "realized_pnl": pnl, "roi_pct": roi, "fees": fees,
            "win": 1 if pnl > 0 else 0,
            "entry_matched": 1 if e_row else 0,
            "exit_source": "script" if x_row else "engine_or_unmatched",
        }
        for f in feats:
            rec[f"entry_{f}"] = e_row.get(f) if e_row else ""
            rec[f"exit_{f}"] = x_row.get(f) if x_row else ""
        out_rows.append(rec)

    if not out_rows:
        sys.exit("No positions in the ledger -- nothing to join.")

    with open(args.out, "w", newline="") as fh:
        w = csv.DictWriter(fh, fieldnames=list(out_rows[0]))
        w.writeheader()
        w.writerows(out_rows)

    # ---- winners vs losers, by direction -------------------------------------
    print(f"\nwrote {args.out}: {len(out_rows)} trades "
          f"({unmatched_entry} without a matched entry decision, "
          f"{engine_exit} without a script exit decision)")
    if unmatched_entry:
        print("  ! unmatched entries usually mean the tolerance is too tight, or the "
              "position opened from an engine action rather than your decision block.")
    if engine_exit:
        print("  ! exits with no decision row are stop-loss / take-profit / trailing "
              "closes -- their entry features are still valid evidence.")

    for direction in ("LONG", "SHORT"):
        grp = [r for r in out_rows if r["direction"] == direction and r["entry_matched"]]
        if not grp:
            continue
        wins = [r for r in grp if r["win"]]
        loss = [r for r in grp if not r["win"]]
        print(f"\n{direction}: {len(grp)} trades | winners {len(wins)} losers {len(loss)}")
        if not wins or not loss:
            print("  (need both winners and losers to compute a delta)")
            continue
        print(f"  {'feature':<16}{'winners':>14}{'losers':>14}{'delta':>14}")
        for f in feats:
            def mean(rs):
                vals = []
                for r in rs:
                    try:
                        vals.append(float(r[f"entry_{f}"]))
                    except (TypeError, ValueError):
                        pass
                return sum(vals) / len(vals) if vals else None
            mw, ml = mean(wins), mean(loss)
            if mw is None or ml is None:
                continue
            print(f"  {f:<16}{mw:>14.4f}{ml:>14.4f}{mw - ml:>14.4f}")
        if min(len(wins), len(loss)) < 5:
            print("  ! fewer than 5 trades on one side -- treat these deltas as noise.")


if __name__ == "__main__":
    main()
```

Run it:

```bash
python3 join_outcomes_cloud.py triggers.csv positions.json --interval 60
```

Real output from a 31-day instrumented run on Cloud:

```
wrote trades_dataset.csv: 10 trades (0 without a matched entry decision,
                                     0 without a script exit decision)

LONG: 10 trades | winners 5 losers 5
  feature                winners        losers         delta
  Price               62032.2860    64596.6800    -2564.3940
  RSI                    31.8220       37.1640       -5.3420
  EMAfast             62955.5380    65192.4140    -2236.8760
  EMAslow             63079.6560    65288.3800    -2208.7240
  TrendUp                 0.2000        0.0000        0.2000
  PosAmount               0.0000        0.0000        0.0000
  ! fewer than 5 trades on one side -- treat these deltas as noise.
```

---

## Reading the result and acting on it

**delta = winners − losers**, computed on the *entry* feature-vector. Its sign and size tell
you where winning trigger conditions sat relative to losing ones.

In the example above, winning long entries fired at a mean RSI of **31.8** while losers fired
at **37.2**. The strategy's buy threshold was 40 — so the losing half of the book was
entering in the 37–40 band, on dips that were not deep enough. That is a directly testable
hypothesis: *tighten the buy threshold and the shallow-dip losers should disappear.*

How to act on it, in order:

1. **Check sample size first.** With ten trades and a five/five split, that delta is a
   hypothesis, not a finding. The script warns you when either side has fewer than five
   trades. Widen the window or accept that you are guessing.
2. **Always read by direction.** Long and short use opposite-sided thresholds; a blended
   mean is meaningless. The script splits them for you.
3. **Form exactly one hypothesis per iteration**, so you can attribute the effect.
4. **Sweep it, don't hand-pick it.** Turn "winners cluster below RSI 32" into a Lab range
   like `RSI Buy Level: range [26, 38, 2]` and let the sweep find the optimum. Hand-picking
   the observed mean overfits it.
5. **Validate across regimes.** Because labs cap at 31 days, that means several one-month
   labs on deliberately chosen regime months — one bear, one bull, one sideways. Use
   `clone_lab` per month so the parameter grid is preserved and only the
   `start_lab_execution` dates change. Rank survivors by **worst-month** ROI, never summed
   ROI, and filter out zero-trade configs first — they score 0.00 and outrank every losing
   config, so they will otherwise occupy the top of your leaderboard.
6. **Confirm standalone** over the longest window your plan allows — 186 days on Standard,
   372 on Pro — and judge the result against **buy-and-hold for the same period**, not
   against zero.
7. **Watch fees.** A threshold change that adds trades can erode net profit even when
   per-trade edge improves. Check fee drag (fees as a share of gross), not just win rate.

The per-trade `trades_dataset.csv` is there for deeper cuts: bucket entries by a feature and
compare win rate per bucket, correlate features with P&L, or filter to a single regime.

---

## Cloud gotchas, collected

- **Missing `orderTemplate` silently breaks every trade action** while the backtest reports
  success. Copy the settings block from a completed lab via `get_lab_details`.
- **A run that errors mid-way still reports `S=3` at 100%.** The export file just stops at
  the failing tick. Always read `get_backtest_logs` before trusting row counts.
- **Per-tick logging fires every minute** even at `interval=60` — budget rows as *minutes in
  the period*, not chart bars.
- **Full float precision doubles your payload.** `Round(x, 2)` on every value cut a measured
  31-day export from 3.44 MiB to 1.93 MiB.
- **Empty export string** = wrong `name`, zero decisions, or an early failure. In that order.
- **`PosAmount` is 0 on entry rows** — the fill isn't booked in the same tick. Expected.
- **Logged price is the decision-bar close, not the fill.** Reconcile on timestamp.
- **Engine-driven exits have no decision row.** They appear in the join with
  `exit_source=engine_or_unmatched`. Their entry features remain valid evidence; don't try
  to tune an exit threshold from them.
- **Spot can't short.** `DoShort` is a no-op — short decisions log but never fill, and a
  naive gate will re-fire every minute. Use margin/futures, and edge-gate regardless.
- **`Save`/`Load` persist across runs.** Reset every instrumentation flag in `Finalize()` or
  a stale value leaks into the first ticks of the next backtest.
- **One backtest per script at a time.** To parallelise, clone the script under a second name.
- **`create_lab` defaults `tradeAmount` to 100.0.** Override it in `update_lab` or lab and
  standalone results are not comparable.
- **Labs cannot exceed 31 days** on any Cloud tier, so cross-regime validation always means
  several one-month labs rather than one long sweep.
- **Live bots are read-only over MCP.** Any change this analysis recommends is applied in
  the HTS web interface.

---

## Quick reference

**"Capture what my entries fire on, over a year."**
→ Decision-event logging only (Pattern A). At ~64 bytes/row it is free at any horizon. A
full year fits one Pro standalone run (372 days); on Standard, split it into two 186-day
windows and retrieve each separately.

**"Log every indicator value on every bar for a month."**
→ 44,641 rows. Round everything (1.93 MiB, retrievable) or gate to interval-aligned bars
(744 rows, trivial). Full precision is 3.44 MiB and will strain most MCP clients.

**"Which parameter set's entries actually look different?"**
→ Run the sweep, then pull `get_backtest_export_data(BID, lab_id=...)` for the top and bottom
combos and compare their entry feature distributions. Uniquely possible on Cloud because lab
exports drill in directly.

**"My export file is empty."**
→ Check the `name` matches `InitExportData` exactly, then `get_backtest_logs` for an
execution error, then whether the strategy made any decisions at all.

**"My export stops partway through the run."**
→ Execution error at that timestamp. `get_backtest_logs` will name it. Ninety percent of the
time on a first Cloud run it is the missing `orderTemplate`.

**"Zero positions but my trigger file has entries."**
→ Same cause: the decisions fired, the orders failed. Or you are shorting on spot.

**"Can I still use the PowerShell retrieval script from the self-hosted workflow?"**
→ No, and you don't need it. Cloud has no host filesystem;
`get_backtest_export_data` returns decoded CSV directly.
