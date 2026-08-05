---
name: haasscript-development
description: >
  End-to-end HaasScript creation, modification, and debugging workflow. Use this skill
  whenever the user wants to write a new trading script, modify an existing one, fix a
  compile error, debug unexpected behaviour, or understand why a script isn't executing
  trades. Triggers include: "write me a script", "add a feature to my bot", "the script
  won't compile", "why are there no trades?", "add a stop loss", "fix this error",
  "create a grid bot", "implement an ICT strategy", "I want a trend filter", or any
  request involving HaasScript code creation, editing, or debugging. Always use this
  skill proactively before writing any HaasScript code — it contains the mandatory lint
  checklist, known gotchas, and compile-debug cycle that prevent the most common failures.
---

# HaasScript Development Skill

**Provenance.** Everything here was empirically verified on HaasOnline Trade Server
(self-hosted Enterprise "HTSLocal" and, where noted, HTS Cloud / HTSv3) between roughly
2026-04 and 2026-08, via compile probes, real backtests, and live bots. Items tagged
**[BUILD]** vary by build/edition — probe them before relying on them. When this document
and a command reference disagree, believe a compile probe.

**Verify signatures, don't guess them.** If a `search_haasscript_docs` tool is available
on the connected server (Cloud and Stage have it; HTSLocal does not), search it before
writing any command you have not personally used. Otherwise compile-probe.

---

## §0 — The six that cause SILENT failure

These do not throw. They produce a clean-looking run with zero or wrong trades. Check
every one of them before you debug anything else.

1. **Execution is PER-MINUTE regardless of `interval`.** `interval` only changes candle
   aggregation. A 15m strategy still runs the script 15 times per bar. Every
   interval-level decision needs a new-bar gate (§7.1) or it fires N times per bar.
2. **Index `[1]` is INTRA-BAR LOOKAHEAD.** `ClosePrices()[1]` carries the forming bar's
   *final* close, and `GetVolume()[1]` carries the forming bar's *complete* volume from
   minute 0. Use `[2]` — the last closed bar — for anything causal. Verified twice
   (volume 2026-07-26, price 2026-08-02).
3. **`settings.orderTemplate` must be 500.** Omit it and the engine crashes at the first
   order that consults the template. Status still reads S=3 / P=100 with PT=0 — it looks
   like a clean zero-trade run unless you read the logs.
4. **`Finalize()` runs ONCE at the end of a backtest but EVERY TICK in a live bot.**
   Any state reset inside `Finalize` is harmless in a backtest and wipes your live bot's
   state 60 times an hour. Finalize reports; it resets nothing live logic depends on.
5. **Loop-variable indexing returns userdata.** `closes[i]` where `i` is a `for` variable
   yields a wrapper that will not do arithmetic. Launder it (§7.3) or use element-wise
   array algebra (§7.4).
6. **Save/Load state persists across backtest runs** on the same script, and `Save()` is
   deferred *within* a tick (a later `Load()` in the same tick returns the tick-start
   snapshot). Reset on a fresh-run guard, not in `Finalize`.

---

## Phase 1 — Clarify intent before writing code

**Always draft an implementation plan and get explicit user approval before touching any
script.** Present it as numbered logic blocks. Extract answers from conversation history
first; only ask what you cannot infer.

| Question | Why it matters |
|---|---|
| Spot or futures/margin? | Different order commands; `DoShort` is a **no-op on spot** |
| Managed or unmanaged? | They **cannot be mixed** — one leftover managed call is a compile error |
| Entry trigger? | Indicators, levels, time, structure |
| Exit geometry: TP / SL / trailing / time? | Never skip. Exit geometry dominates entry quality in every measured comparison |
| Swept in Labs later? | All sweepable `Input()` booleans need `1`/`0` defaults |
| New script or modify existing? | Existing → clone a named backup first |
| Custom report metrics? | Plan `CustomReport()` upfront — and see the counter caveat in §3 |
| Will it be tuned from evidence? | If yes, add the decision-edge capture block now (**haasscript-export-instrumentation** skill) |

---

## Phase 2 — Code structure template

```lua
-- =============================================================
-- StrategyName-vX.Y.Z-descriptor
-- Brief: one line on what this bot does
-- =============================================================

-- ── INPUTS (all at TOP LEVEL, above anything that can throw) ──
local emaLen   = Input("EMA Length", 20, "Trend EMA period")
local useFilt  = Input("Enable Filter", 1, "1=on, 0=off")   -- NEVER true/false
local tfMins   = Input("Trigger TF Minutes", 15)

-- ── HELPERS ──────────────────────────────────────────────────
-- double-domain launder: bridge Decimal/userdata -> native Lua double
local function lau(v)
    if v == nil then return nil end
    local ok, r = pcall(function() return tonumber("" .. v) end)
    if ok then return r end
    return nil
end

-- ── NEW-BAR GATE (mandatory for interval-level decisions) ────
local barSecs = tfMins * 60
local ts      = lau(Time())
local barId   = lau(Floor(ts / barSecs))
local isNewBar = barId ~= lau(Load("lastBar", -1))
if isNewBar then Save("lastBar", barId) end

-- ── DATA (causal index = [2], NOT [1]) ───────────────────────
local cl = ClosePrices()
local hi = HighPrices()
local lo = LowPrices()
local c2 = lau(cl[2])

-- activation pass feeds a synthetic flat series (close=100, ATR=0)
if c2 == nil or c2 == 100 then return end

-- ── INDICATORS ───────────────────────────────────────────────
local ema  = EMA(cl, emaLen)
local ema2 = lau(ema[2])
if ema2 == nil or ema2 <= 0 then return end

-- ── POSITION TRUTH (never a saved flag) ──────────────────────
local pos      = GetPositionDirection()
local flat     = pos == NoPosition
local isLong   = pos == PositionLong
local isShort  = pos == PositionShort

-- ── DECISIONS (gated on the new bar) ─────────────────────────
if isNewBar then
    if flat and not IsAnyOrderOpen() and c2 > ema2 then
        DoLong("Above EMA")
    elseif isLong and c2 < ema2 then
        DoExitPosition("Lost EMA")
    end
end

-- ── FINALIZE: REPORTS ONLY. RESETS NOTHING. ──────────────────
Finalize(function()
    CustomReport("Bars Seen", Load("barCount", 0))
end)
```

**Fresh-run detection** (replaces resetting state in `Finalize`): a new backtest starts
*before* the last saved tick's timestamp; live time never reverses.

```lua
local prevTs = lau(Load("prevTs", 0))
if prevTs ~= nil and ts < prevTs then
    -- new backtest run: clear counters/flags here
end
Save("prevTs", ts)
```

Bar-gap heuristics do **not** work for this — HTS ticks at variable sub-bar intervals
during warmup.

---

## Phase 3 — Pre-compilation lint checklist

### 🔴 Silent-failure class

- [ ] **New-bar gate wraps every interval-level decision.** Without it a 15m signal fires
      ~15× per bar. Proven: 3 entries off one 5-minute bar.
- [ ] **Gate is time-derived, not bar-count-derived.** `#ClosePrices()` is a CONSTANT
      fixed-depth window (500 in backtests, 1000 at compile-test) — it never grows, so
      `#ClosePrices() ~= Load("prevCount")` fires once at activation and never again.
      Use `Floor(Time() / barSecs)`.
- [ ] **Causal data uses `[2]`.** `[1]` is the forming bar with its final close and full
      volume already visible.
- [ ] **Boolean `Input()` defaults are `1`/`0`.** `true`/`false` breaks Lab sweeps with
      "Cannot parse '1' to a boolean value". `InputOptions()` returns a STRING — keep
      swept parameters numeric.
- [ ] **All `Input()` calls at top level, above anything that can throw.** Inputs register
      as each line executes on the init tick; an earlier runtime crash silently aborts
      registration of every later input. Inputs inside `if` branches get only a
      static-scan fallback key.
- [ ] **Array indexing starts at `1`.** Index 0 throws.
- [ ] **Every `Load()` key has a matching `Save()`.** Mismatched keys silently return the
      default forever.
- [ ] **`Finalize` resets nothing that live logic depends on** (it runs every tick live).
- [ ] **Managed XOR unmanaged.** Never both — a leftover `if dummy then DoLong(...) end`
      is enough to trip "Both managed and unmanaged trading commands detected".
- [ ] **Unmanaged order calls are PRICE-FIRST.** `PlaceBuyOrder(price, amount)`,
      `PlaceGoLongOrder(price, amount)`. The `(amount, price)` form **places nothing**:
      0 orders, 0 fills, empty FailedOrders.

### 🟡 Type-system class

- [ ] **No `math.*`.** Not available. Use `Abs`, `Round`, `Max`, `Min`, `Floor`, `Ceil`,
      `Sqrt`, `Ln`, `Atan`.
- [ ] **`Round(x, digits)` takes TWO args.** The 1-arg form crashes with
      "Invalid input count of 1". `Round()` on a **bool hard-errors and is NOT
      pcall-catchable** — test `== true` first.
- [ ] **`Sqrt(x)`, never `x ^ 0.5`.** `^` crashes on bridge-derived values.
- [ ] **`Ln()` bounds-guarded** — throws "too large or too small for a Decimal" near 0 or
      on huge/NaN input.
- [ ] **Collection scalars coerced.** `arr[1]` is not automatically a clean number —
      `+ 0` for simple cases, `lau()` for anything feeding recursion or a multiply.
- [ ] **No loop-variable indexing arithmetic.** `closes[i]` with a `for` variable returns
      userdata. Launder, or use element-wise array ops (§7.4).
- [ ] **Loop bounds are literals, `Input()` values, or `#collection`.** `Min()`, `Max()`,
      `Floor()` and numeric `Load()` returns are userdata and are **rejected as `for`
      limits** ("'for' limit must be a number"); `+ 0` does not fix it. A userdata control
      variable also makes `string.format` throw, which kills string-keyed tables.
- [ ] **`type()` is unregistered** [BUILD]. Use `pcall` guards, `nil` checks and `#`.
- [ ] **`tostring()` — avoid.** Fine on HTSLocal; the **live HTSv3 editor rejects it**.
      Use implicit `..` coercion. Note `..` binds looser than `+ -`.
- [ ] **Never string-coerce a whole collection.** It dumps the entire series into the log
      or order note and trips the >5000-char warning.
- [ ] **Never `Save()` a container.** `Save(CurrentPrice())` crashes the run.
      `Save(nil)` throws.
- [ ] **Decimal precision:** `Round(x, 8)` after every divide; launder every value feeding
      a recursion or an accumulator (§7.3).

### 🟢 Practice

- [ ] `HideOrderSettings()` / `HideTradeAmountSettings()` when the script sizes its own orders.
- [ ] Sizes derived from `MinimumTradeAmount(market, price)`, `IsTradeAmountEnough(...)`,
      `AmountStep(market)` — not hardcoded minimums.
- [ ] Version tag in the script name: `StrategyName-vX.Y.Z-descriptor`.
- [ ] `Log()` concise and conditional — verbose logging floods and hides real signals.
- [ ] Supported colours only: `Aqua Black Blue Cyan DarkGray DarkGreen Fuchsia Gold Gray
      Green Maroon Olive Orange Purple Red SkyBlue Teal White Yellow`.
- [ ] Source is **ASCII-only** — `add_script` strips non-ASCII.

---

## Phase 4 — MCP tool surface and the compile → debug cycle

The tool names are `add_script`, `compile_script`, `edit_script_source`,
`execute_backtest`. There is **no** `compile_new_script`, `compile_existing_script`,
`run_backtest`, `compare_backtests`, or `batch_backtest` — compare runs by reading
`get_backtest_info` / `get_backtest_runtime` on each and diffing the fields yourself.

| Need | Call |
|---|---|
| Syntax check without saving | `compile_script(source_code, market_tag, interval, account_id, trade_amount)` |
| Create | `add_script(name, script, type="Lua", ...)` → returns `SID` |
| Edit in place (keeps backtest history) | `edit_script_source(script_id, source_code, settings)` |
| Read back / get input keys | `get_script(script_id)` → `CR.IV`, `CR.CL`, `CR.I[]` |
| Run | `execute_backtest(script_id, backtest_id, start_unix, end_unix, settings)` |
| Poll | `get_backtest_status(backtest_id, service_id)` → `S=3`, `P=100` when done |
| Logs / metrics / ledger / rows | `get_backtest_logs`, `get_backtest_info`, `get_backtest_runtime`, `get_backtest_positions`, `get_backtest_export_data` |
| Data pre-flight | `check_market_data(market, start_unix, end_unix, interval)` |

### The settings object — every field matters

```json
{
  "accountId":  "<guid>",
  "marketTag":  "BINANCEFUTURES_BTC_USDT_PERPETUAL",
  "interval":   15,
  "tradeAmount": 0.003,
  "chartStyle": 300,
  "orderTemplate": 500,
  "leverage": 1,
  "positionMode": 0,
  "marginMode": 0,
  "scriptParameters": { "<EK>.<Input Name>": "1" }
}
```

- **`orderTemplate: 500` is mandatory** (§0.3).
- **`backtest_id` has a minimum length** — short ids fail with a misleading
  "Invalid backtestId". Use ~15+ descriptive characters.
- **One backtest per script at a time** — "Cannot execute multiple backtests of the same
  script at once". To parallelise, clone the script under different names.

### `scriptParameters` DO work — with one trap

Per-backtest `Input()` overrides work through `settings.scriptParameters`, keyed
`"<EK>.<Name>"`, values as **strings**. Get the key from `get_script` → `CR.I[]`
(`K` = full key, `EK` = prefix, `N` = name, `D` = default).

**The key prefix is POSITION-DEPENDENT.** `137-137-27-32` encodes where the `Input()` call
sits in the source — adding, removing or reordering any Input above it shifts every
subsequent key. Re-read `CR.I` after any script edit; never hardcode a key across versions.

**Always verify the override applied**: `get_backtest_runtime` → `InputFields[<key>].V`
versus `.D`. An unrecognised settings key is silently ignored and the run proceeds at
defaults. Best practice: echo the value into the export and check it there.

### Compile-log traps

- The compile harness runs a **synthetic flat series** (close=100, ATR=0, volume=100).
  So volume-dependent commands (`OBV`, `MFI`, `AD`) collapse to 0 in compile output —
  a placeholder, not a failure. **Validate volume and orderbook logic in a real backtest,
  never from compile output.**
- Guard the activation pass in code: `if atr[2] <= 0 then return end` or
  `if c2 == 100 then return end`.
- The lenient API-level compile check **tolerates `tostring`** while the live HTSv3 editor
  rejects it. Grep for `tostring` and `math.*` regardless of what the compiler says.
- Signature errors surface at **runtime**, not compile. An invented positional order form
  is valid Lua and compiles, then throws "Argument out of range" at order-build time.

### Compile error → fix

| Error | Cause | Fix |
|---|---|---|
| `Cannot parse '1' to a boolean value` | `Input()` boolean default is `true`/`false` | Use `1`/`0` |
| `'for' limit must be a number` | Loop bound from `Min`/`Max`/`Floor`/`Load` | Literal / `Input()` / `#collection` bound |
| `attempt to perform arithmetic on a userdata value` | `closes[i]` with a loop variable | Launder (§7.3) or element-wise ops (§7.4) |
| `Value was either too large or too small for a Decimal` | Mantissa-precision overflow in a recursion or multiply | Launder to doubles; `Round(x,8)` after divides |
| `Invalid input count of 1` | `Round(x)` with one arg | `Round(x, digits)` |
| `Both managed and unmanaged trading commands detected` | A stray `DoLong`/`DoExitPosition` in an unmanaged script | Remove it |
| `Unknown references: <name>` | Command not registered on this build | Probe alternatives; see §8 |
| Compiles, zero trades | Config or gating | Phase 5 |

---

## Phase 5 — Debugging zero-trade backtests

Work the tiers in order. Tier 0 is new and catches the most confusing case.

### Tier 0 — Read the logs even when the run "succeeded"

```
get_backtest_logs(backtest_id)
```

A missing `orderTemplate` crashes at the first order while status still reads S=3 /
P=100 / PT=0. The log shows `Backtest has failed. There is a execution error.` /
`Argument out of range exception.` It is script-, market-, account- and window-independent,
and scripts that pass an explicit order type (`{type = MarketOrderType}`) survive — which
falsely implicates the crashing scripts. Diagnose by comparing
`get_backtest_runtime(...).OrderTemplate` between a passing and a failing run.

### Tier 1 — Data and configuration

| Log string | Meaning | Fix |
|---|---|---|
| `is syncing` / `Price data of <MKT> is syncing` | Market not ready. **Fatal — not nil-able and not pcall-able**; it kills the whole run on the first tick | `sync_market_data`, then retry. A multi-market universe must contain only pre-synced markets |
| `Out of Sync` | Dynamic base price reset | Review re-anchor logic |
| `Backtest has failed` | Execution crash | `get_backtest_logs` for the throwing line |

`get_backtest_runtime` — verify `InputFields` (did overrides apply?), `OrderTemplate`,
`positionMode` / `marginMode`, `open_orders` / `failed_orders`.

### Tier 2 — Script logic

- **No new-bar gate, or a broken one** (`#ClosePrices()` bar-count gate → fires once).
- **Trade size below the exchange minimum** — silently rejected every tick with nothing
  surfaced. Check with `IsTradeAmountEnough` and log rejections.
- **Leverage**: futures accounts default very high (125–150×). Pass `leverage` explicitly.
  Note the backtest `leverage` setting does **not** propagate to non-primary markets in
  multi-market scripts — those use the account's per-market default.
- **Order args reversed** — `(amount, price)` places nothing, silently.
- **Plain limit at the close does not fill** — the close can sit below the ask after a
  bounce. Use `{type = MarketOrderType}` or price it marketable (`close * 1.02` to buy).
- **Position guard wedged** — `not IsAnyOrderOpen()` never clears if an order is stuck.
- **`GetPositionDirection()` compared to raw numerics** — `0`/`-1`/`1` never match the
  enum. Compare to `NoPosition` / `PositionLong` / `PositionShort`.
- **Spot `DoShort` is a no-op** — logic waiting for a short position to appear will wedge.
- **Managed-spot fee dust**: spot fees are charged in the base asset, so `DoExitPosition`
  sells slightly less than was bought and leaves ~1e-5 dust. The engine keeps the position
  "open", so `posDir == NoPosition` entry gating freezes the bot after one round trip.
  Robust fix: go unmanaged for spot strategies needing clean discrete entries.

### Diagnostic logging

```lua
Log("c2=" .. Round(c2, 2) .. " ema2=" .. Round(ema2, 2)
    .. " newBar=" .. (isNewBar and 1 or 0), Orange)
```

No `tostring()`; no whole-collection concatenation. Note that on a crash `Log()` flushes
reliably only to a *region* — the throw swallows the last unflushed chunk. For an exact
bisect, use an early-exit probe (`do return end`) on the suspect bar rather than trusting
the last log line.

---

## Phase 6 — Version control and regression

**Mandatory for any breaking change.**

1. Clone a named backup at the current state before editing.
2. `edit_script_source(script_id, new_code, settings)` — modifies in place, preserves
   backtest history.
3. Re-run the **exact same** window, account, interval, leverage, position/margin mode as
   the last known-good run.
4. Diff `get_backtest_info` / `get_backtest_runtime` between the two runs yourself.

**Thresholds.** For an intended behaviour change: ROI ±0.5%, trades ±2, PF ±0.05. For an
*inert* change — a new mode defaulting to off, or added instrumentation — the gate is
**bit-identical**: same ROI, same trade count, same entry/exit prices. Anything else means
a Save-key collision or a logic side-effect.

**Ship inert.** New behaviour goes behind a numeric mode `Input()` whose default reproduces
the incumbent exactly. It makes the regression gate trivially provable and turns the mode
into a sweepable Lab dimension.

**A fix that changes nothing means nothing was broken.** If a "bug fix" produces a
bit-identical run, the bug was not there — go find the real cause instead of banking the fix.

**Live bots do not hot-reload.** Deactivate and reactivate to pick up a recompiled script.
Bot-level Input overrides persist across script edits — changing a source default does not
change a configured bot.

---

## Phase 7 — Verified patterns

### 7.1 New-bar gate

```lua
local barSecs = tfMins * 60
local ts      = lau(Time())
local barId   = lau(Floor(ts / barSecs))
local isNewBar = barId ~= lau(Load("lastBar", -1))
if isNewBar then Save("lastBar", barId) end
-- wrap arming + entry/exit DECISIONS in `if isNewBar then ... end`
-- run fill-detection and exit checks EVERY tick
```

### 7.2 Causal data access

```lua
local cl, vc = ClosePrices(), GetVolume()
local c2 = lau(cl[2])    -- last CLOSED bar
local v2 = lau(vc[2])    -- its complete volume
-- cl[1] / vc[1] = the forming bar, already carrying its final values = LOOKAHEAD
```

Anchor price-based TP/SL on the **live fill price** (`ClosePrices()[1]` at the confirmation
tick), not the stale closed bar — a gap between the two causes instant stop-outs.

### 7.3 Double-domain launder

The definitive fix for the userdata wall and Decimal precision faults. The string
round-trip converts bridge Decimals to native Lua doubles (~15 significant digits; BTC
needs ~9, so precision is a non-issue).

```lua
local function lau(v)
    if v == nil then return nil end
    local ok, r = pcall(function() return tonumber("" .. v) end)
    if ok then return r end
    return nil
end
```

Launder every price container, `Input()`, indicator scalar and numeric `Load()` return
once per tick into plain tables, then do all downstream arithmetic on doubles. Magnitude
clamps, `Round(x,8)` alone, and add/subtract laundering do **not** clear the taint;
Save/Load round-trips preserve it.

### 7.4 Element-wise array algebra — prefer this over loops

`Add`, `Sub`, `Mul`, `Div`, `Abs`, `Clamp`, `Pow`, `Exp` take **and return**
`HaasNumberCollection`. With `Offset` and `Grab`, a whole Pine-style series expression
becomes engine vector ops with **zero Lua indexing** — no userdata wall, no ring buffers.

- Index `1` = newest.
- `Offset(a, n)` strips the **newest** n elements.
- `Grab(a, offset, count)` — offset is a **0-based skip from the newest end**.
- `Add`/`Sub` align on the NEWEST element and truncate to the shorter input.
- `SMA`, `EMA`, `STDDEV`, `ATR` accept **derived** collections. `STDDEV`'s 3rd arg is a
  sigma multiplier (1 = plain sigma).
- `GetHigh(arr, depth, offset)` / `GetLow(arr, depth, offset)` are **userdata-free element
  accessors** → stateless pivot scans with no Save/Load.

Worked example — Kaufman Efficiency Ratio as a full series, verified exact against the
scalar form (delta 0.000000000):

```lua
local chg     = Abs(Change(cl))                    -- |dclose| series
local pathSer = Mul(SMA(chg, 20), 20)              -- rolling 20-sum
local netSer  = Abs(Sub(cl, Offset(cl, 20)))       -- |close - close[20]|
local kerSer  = Div(netSer, pathSer)               -- KER series, [2] = causal
-- scalar equivalent: ArraySum(Grab(chg, 1, 20)) skips the forming bar and spans cl[2]..cl[22]
```

For a median or percentile, launder a slice into a plain Lua table and `table.sort` it —
native Lua tables work, and `Save`/`Load` persists them (ring buffers are fine).

### 7.5 The 500-bar window

`ClosePrices()` and friends are a fixed 500-bar array recomputed each tick (1000 at
compile-test). Consequences:

- Long-period EMAs re-seed ~450 bars back and never fully converge — a naive parity check
  against a reference implementation shows its largest error on the longest-period EMA.
  Emulate the whole per-tick stack rather than one indicator.
- **The engine PRE-LOADS ≥500 bars from before the backtest start**, so a 200-bar
  statistic is available on the very first bar — no warm-up pad needed on deep history.
  On a market whose data begins recently, leave ~7 days before the start.
- Anything needing more than ~498 bars since an anchor (e.g. a month-anchored accumulator
  on 15m or 1h) does not fit — move to a higher timeframe or accumulate statefully.

### 7.6 Unmanaged single-pair state machine

```
place limit -> fill-detect (GetPositionAmount(id) > 0) -> track MFE via Save/Load
   -> TP/SL check -> MARKETABLE close -> flat-detect -> book the trade
```

New-bar gate the decisions; run fill and exit checks every tick.

- **Validate the order id returned by every `Place*` call.** A failed placement can return
  an empty id; persisting `""` with a direction set creates a zombie slot that wedges
  side-counting logic forever. On failure keep the slot in a retry state with backoff.
- **Native `StopLoss(pct)` / `TakeProfit(pct)` / `TrailingStopLoss(pct)` return BOOL** —
  they are **detectors, not executors**. They latch true and re-fire every tick once
  breached; gate the close on an already-acted flag and place the close yourself.
- **Price closes marketable.** A stop placed as a plain limit at the trigger price is
  non-marketable in a fast adverse move and never fills. Long close = sell at
  `price * (1 - slip)`; short close = buy at `price * (1 + slip)`.
- **Derive "am I in a trade" from `GetPositionAmount(...)`, never from a saved flag.**
  Flags desync and die under live `Finalize`; position truth is immune.

### 7.7 Hedge — simultaneous long and short

Works with **unmanaged** orders using a **distinct `positionId` per slot**. Slots with
separate ids are independent and do not net; managed `DoLong`/`DoShort` take no positionId
and still flip. Per-slot state: `GetPositionAmount("L")`, `StopLoss(pct, "L")`,
`AverageEnterPrice("L")`, `GetPositionROI("L")`.

**Gate slot open/flat on `GetPositionAmount(id) > 0`, not `GetPositionDirection(id)`** —
direction lingers as the last side after a close, so a direction-based flat check sticks
and the slot never re-enters.

### 7.8 Limit-entry with timeout

Place a passive limit at `close ± offset%`; count bars via the new-bar gate;
`CancelAllOrders()` after N bars unfilled. **Maker execution roughly halves fee drag**
versus taker and is repeatedly the difference between net-negative and net-positive.

### 7.9 Session filter

```lua
local hour = CurrentHour()   -- UTC
if not (hour >= 13 and hour < 16) then return end
```

---

## Phase 8 — Command reference

### Verified working

```lua
-- Price data (collections, newest-first; optional interval/fullCandles/market/hlcStyle)
ClosePrices()  OpenPrices()  HighPrices()  LowPrices()
HLCPrices()  HLPrices()  OCPrices()  OHLCPrices()  SourcePrices(type)
BuyPrices()/AskPrices()   SellPrices()/BidPrices()
GetVolume()                      -- REAL per-bar exchange volume
CurrentPrice([market])           -- CONTAINER {open,high,low,close,volume,ask,bid}; never Save() it

-- Multi-timeframe / multi-market
ClosePrices(240)                 ADX(HighPrices(240), LowPrices(240), ClosePrices(240), 14)
ClosePrices(interval, false, market)        CreateMarket("", "ETH", "USDT", "PERPETUAL")
PriceMarket()                    -- current market tag

-- Indicators (return collections; [1] newest)
RSI(src, len)   EMA(src, len)   SMA(src, len)   KAMA(src, period)
MACD(src, 12, 26, 9)             -- [1]=line [2]=signal
BBANDS(src, n, upMult, dnMult)   -- NOT BollingerBands
ATR(h, l, c, n)   NATR(...)   ADX(h,l,c,n)   CHOP(h,l,c,n)   -- 4-input HLC forms
AROON(h, l, n)                   -- [1]=DOWN series, [2]=UP series
WILLR(h, l, c, n)   MFI(h,l,c,vol,n)   OBV(closes, vol)   AD(h,l,c,vol)   ADOSC(...)
KELTNER(h, l, c, emaN, atrN, mult)
CDL(o, h, l, c, EngulfingType)   -- *Type enums are userdata CONSTANTS, no parentheses

-- Math / arrays
Abs Round(x,d) Max Min Floor Ceil Sqrt Ln Log10 Exp Pow Sign Clamp Truncate
Add Sub Mul Div  Average Average2 Change Compress Delta Sd
GetHigh(arr,depth,offset)  GetLow(...)  GetHighs  GetLows
ArraySort ArraySum ArrayIndex ArrayLast ArrayAdd Count Grab Offset SourceManager

-- Time
Time()            -- current Unix timestamp
CurrentHour() CurrentMinute() CurrentDay() CurrentDate() CurrentWeek() CurrentMonth() CurrentYear()
CreateTimestamp(...)  AdjustTimestamp(...)  OpenTime(unix, interval)  CloseTime(...)
MinutesTillCandleClose(interval)  FormatDateTime(...)

-- Managed trading
DoLong(note) DoShort(note) DoExitPosition(note) DoFlipPosition(note) DoSignal(sig)
SignalLong/SignalBuy  SignalShort/SignalSell  SignalExitPosition  SignalNone
EasyRSI(...) EasyMACD(...) EasyEMA(...) EasyOBV(...) EasyMFI(...)   -- return signal enums
IndicatorContainer(s1,s2,s3)     -- [2] unanimous, [3] majority

-- Unmanaged trading  (PRICE FIRST)
PlaceBuyOrder(price, amount [, opts])     PlaceSellOrder(price, amount [, opts])
PlaceGoLongOrder(price, amount, ...)      PlaceGoShortOrder(price, amount, ...)
PlaceExitPositionOrder(positionId, price, type, note)
PlaceExitLongOrder(price, amount, market, template, note)   -- [BUILD] probe availability
CancelOrder(id)  CancelAllOrders()
MarketOrderType  LimitOrderType          -- userdata constants, NO parentheses
-- TRAP: in the long positional form, arg3 is a MARKET STRING (use PriceMarket()).
-- Passing a bare number there sets market="500" and fails at runtime with a null-ref.
-- Builds differ on whether arg4 is an order-type constant or a numeric template id — PROBE.
-- The 2-arg form (price, amount) always works and defaults market/template.
-- Options-table form also works: PlaceGoLongOrder(price, amount,
--   {positionId=..., market=..., note=..., type=MarketOrderType, timeout=...})

-- Position / P&L
GetPositionDirection()   -- vs NoPosition | PositionLong | PositionShort
GetPositionAmount([id])  GetPositionProfit()  GetPositionEnterPrice()
GetPositionROI([id])     AverageEnterPrice([id])   GetCurrentROI(directionConstant)
GetCurrentProfit([dir][, market])         -- net of fees
GetBotProfit()  GetBotROI()               -- engine NET realized; ties to the order ledger
IsAnyOrderOpen()  GetOrderProfit(id)  GetOrderFilledAmount(id, false)
StopLoss(pct[,id])  TakeProfit(pct[,id])  TrailingStopLoss(pct[,id])   -- BOOL DETECTORS

-- Sizing
MinimumTradeAmount(market[, price])  IsTradeAmountEnough(market, price, amount)
ParseTradeAmount(...)  AmountStep(market)  PriceStep(market)  AddPerc/SubPerc

-- Wallet
WalletAmount(AccountGuid(), "USDT")  BaseCurrency(m)  QuoteCurrency(m)  AccountGuid()

-- Orderbook
GetOrderbook([market])   -- ob[1] askPrices, ob[2] askAmounts, ob[3] bidPrices, ob[4] bidAmounts
                         -- named keys (.bidPrices etc.) also resolve on later builds — PROBE
-- NOTE: backtests have NO historical orderbook. GetOrderbook() is synthetic in BT.

-- Charting
Plot(chartId, name, value [, color|LineOptions])   -- returns a line GUID
PlotBands(g1, g2, fill)  PlotCloud(g1, g2, opacity)  PlotHistogram(g, color2)
PlotShape(...)  PlotBars(guid, base, fill)  PlotDoubleColor(...)  PlotVolume(...)
PlotHorizontalLine(...)  PlotVerticalLine(chartId, name, color, unix)      -- [BUILD]
MarkCandle(chartId, depth)  LineOptions(...)  ChartSetOptions(...)

-- Export instrumentation
InitExportData(name, h1, h2, ...)   WriteExportData(name, v1, v2, ...)

-- Persistence / reporting / control
Save(k, v)  Load(k, default)  CustomReport(label, value)  Finalize(fn)
Log(msg [, Colour])  LogError(msg)  DeactivateBot(reason)
HideOrderSettings()  HideTradeAmountSettings()  EnableHighSpeedUpdates()
Input(name, default, tooltip)  InputGroupHeader("Section")
```

### Unregistered — do NOT use (all threw "Unknown references") [BUILD]

`StdDev` · `BollingerBands` · `SuperTrend` · `EasySuperTrend` · `EasyParabolicSAR` ·
`VWAP` · `Volumes` · `GetVolumes` · **`SetStopLoss`** · **`SetTakeProfit`** · `Pi` · `AMA` ·
`PlotColor` · `PlotLine` · `SetPlotColor` · `PlotArea` · `CurrentUnix` · `Market` (global) ·
`BidAmounts` · `AskAmounts` · `GetOrderbookDepth` · `GetOrderbookLevel` · scalar
`BidPrice`/`AskPrice`/`Bid`/`Ask` · `math.*` · `type()` · `tostring()` (HTSv3 only)

**Substitutions that work:** stdev → hand-roll a rolling Sum/SumSq window
(`var = E[x²] − E[x]²`) for true Pine `stdev(x,n)` parity, or `BBANDS(src,n,1,1)` upper −
mid if the series' own mean is acceptable · Bollinger → `BBANDS` · Supertrend → ATR bands
+ Save/Load flip state · VWAP → hand-rolled anchored `Σ(tp·v)/Σ(v)` · volume → `GetVolume()` ·
stop/target → the BOOL detectors above, or your own price comparison.

### Known-bugged

- **`AmountDecimals(market, amount)` returns PRICE decimals, not amount decimals** —
  verified and reported upstream. `Round(baseAmt, AmountDecimals(...))` silently truncates
  small base amounts to zero on high-priced markets. Derive from `AmountStep(market)`
  instead. `PriceStep`/`PriceDecimals` are correct.
- **`PlotBars(guid, base, fill)` takes a Plot GUID, not standalone args** — the wrong
  arity compiles but runtime-fails the entire script (0 trades).
- **`Plot()` has no per-bar colour.** A named line is one colour for its life; per-tick
  colour swapping renders solid. A direction-coloured line needs a new uniquely-named line
  per direction change (which accrues one line object per flip in a long-lived live bot).
  `Plot` also rejects nil.
- **Save/Load counters read back as defaults inside `Finalize`/`CustomReport` in
  backtests** — so backtest CustomReport counts are unreliable. The engine report, the
  per-position ledger and the export rows are the truth. CustomReport is fine for LIVE bots.
- **HTS's performance block disagrees with its own ledger** (PF and ROI can differ).
  **Quote the ledger.**

---

## What the backtest does NOT model

Treat backtest ROI as an optimistic ceiling.

No orderbook history · no slippage · no partial fills · no latency · no funding rates on
perps · no exchange rejections (min-notional, dust, rate limits appear only live) ·
limit orders fill if the candle range touches them, ignoring queue position (which
**over-fills passive and grid strategies**) · per-minute execution.

HTS also auto-deposits 100k into a simulated wallet when a backtest's balance is
insufficient, so wallet-based capital math in backtests is unreliable unless order sizes
are small enough not to trigger it.

---

## Strategy-engineering rules that survived contact

Not syntax, but they shaped every profitable script here.

- **Fee drag is the primary profitability killer.** Gross-positive signals repeatedly went
  net-negative with fees at 1.7–4× gross. The levers that work: fewer trades, bigger gross
  per trade (wider TP), maker/limit execution, higher timeframe. Tightening exits or adding
  entries usually makes it worse.
- **Check gross/trade before blaming fees.** A filter that looks like a fee cut may be a
  gross-edge selector — identical fees/trade with gross/trade moving −0.065 → +2.235 is
  signal quality, not frequency.
- **Exit geometry dominates entry sophistication.** Inverted R:R (tight TP, wide SL) is a
  structural loser needing ~90% win rate to break even.
- **Leverage multiplies return and risk, not edge** — same Sharpe, ×N ROI and drawdown,
  byte-identical trades. A strategy must be net-positive at 1× first.
- **Validate on long continuous windows**, not stitched favourable regimes. Rank by
  realised + mark-to-market and max drawdown, never realised ROI alone — parked open losses
  masquerade as wins.
- **One lever per iteration**, regression-tested against a known-good window.
- **A winner that wins by near-silence is degenerate**, not an improvement. Check the trade
  count moved sanely before banking a result.
- **Instrumentation must be metric-neutral** — adding it must leave ROI and trade count
  bit-identical. If metrics move, you have a Save-key collision or a logic side-effect.

---

## Output format for a completed script

1. Script name (`StrategyName-vX.Y.Z-descriptor`)
2. One-paragraph plain-English summary of the logic
3. Input parameters table — name, default, purpose
4. Lint checklist result — confirm the 🔴 items explicitly
5. The code
6. Suggested first backtest — which window and account, and what would falsify it

After compiling, confirm `CR.IV = true` via `get_script(script_id)` and state the SID.
