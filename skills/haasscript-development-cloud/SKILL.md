---
name: haasscript-development-cloud
description: >
  End-to-end HaasScript creation, modification, and debugging workflow for
  HaasOnline Cloud via the HaasOnline MCP connector. Use whenever the user wants
  to write a new trading script, modify an existing one, fix a compile error,
  debug unexpected behaviour, or understand why a script isn't executing trades
  on a Cloud server. Triggers include "write me a script", "add a feature to my
  bot", "the script won't compile", "why are there no trades?", "add a stop
  loss", "create a grid bot", "implement an ICT strategy", or any request
  involving HaasScript code creation, editing, or debugging. Contains the
  mandatory pre-flight, the lint checklist, the Cloud MCP tool surface (which
  differs from self-hosted TradeServer), the licence-tier execution caps
  (31-day Labs window, plan-capped standalone backtests), and the
  orderTemplate setting that silently breaks every hand-rolled backtest.
---

# HaasScript Development — HaasOnline Cloud Edition

## Purpose

Guide the full lifecycle of HaasScript creation and debugging on a **HaasOnline
Cloud** server: strategy intent → implementation plan → code draft →
pre-compilation lint → pre-flight → compile → backtest → regression test →
debug. Skipping steps causes silent failures that are hard to trace — on Cloud
especially, because a misconfigured backtest reports success at 100% while
having executed nothing.

This edition is written for **any HaasOnline customer** using the Cloud MCP
connector. It contains no server-specific IDs, account references, or dates.
Resolve every identifier at runtime with `list_scripts`, `list_accounts`,
`list_markets`, `list_labs`, `list_bots`. Numbers marked *(verified)* were
confirmed by live probing against a production Cloud server in July 2026;
numbers marked *(confirmed)* match the server's own enforcement logic. Limits
can change between releases — re-probe on your own server, it is free (see §1.3).

Companion skills: **haasscript-performance-analysis-cloud** (interpret the
results), **haasscript-export-instrumentation-cloud** (capture per-decision
feature vectors for tuning).

---

## 0. Who this applies to, and what you can do

### 0.1 Tier gate

Per HaasOnline's published plan comparison (haasonline.com/pricing, July 2026),
three features this workflow depends on are **excluded from Starter**:

| Feature | Starter | Standard | Professional | TradeServer |
|---|---|---|---|---|
| **MCP Server** | — | ✅ | ✅ | ✅ |
| **HaasLabs** | — | ✅ | ✅ | ✅ |
| **Export Settings & Data** | — | ✅ | ✅ | ✅ |
| Manual Trading | — | ✅ | ✅ | ✅ |
| Trade Bots | 3 | 8 | 20 | 100+ |
| Max Orders Per Bot | 15 | 25 | 50 | 150 |
| Dashboards | 3 | 10 | 25 | 50+ |
| Tick Interval | 10 s | 10 s | 10 s | 5 s |
| Exchanges | 24 | 24 | 24 | 24 |
| **Backtesting** (standalone depth) | 1 month | 6 months | 12 months | 36 months |
| **Max Test Depth** (Labs window) | — | 1 Month | 1 Month | 12 Months |
| Paper / Live Trading | ✅ | ✅ | ✅ | ✅ |

Cloud-hosted tiers are **Starter, Standard, Professional**. **TradeServer is
self-hosted or managed**, not Cloud.

Two consequences that drive everything below:

1. **If you are connected over MCP to a Cloud server, you are on Standard or
   Professional.** Starter has no MCP Server entitlement.
2. **Therefore your Labs window is always 31 days.** The 12-month Max Test Depth
   belongs to TradeServer, which is not a Cloud tier. Do not plan a 3-month
   sweep; it cannot run.

If the user reports they are on Starter, tell them plainly: script development
over MCP requires Standard or above, and Labs/export-based tuning requires the
same. Everything else in this skill assumes Standard or Professional.

### 0.2 Cloud MCP tool surface — names differ from older/local guides

Older HaasScript guidance (and self-hosted setups) references tool names that
**do not exist on the Cloud MCP**. Use the right-hand column.

| If you were told to call… | On Cloud MCP, actually call | Note |
|---|---|---|
| `compile_new_script(code, name, tag)` | `add_script(name, script, type)` then `edit_script_source(script_id, source, settings)` | `add_script` saves; `edit_script_source` recompiles with settings bound |
| `compile_existing_script(id, code)` | `edit_script_source(script_id, source_code, settings)` | Preserves script ID and backtest history |
| *(syntax check only, no save)* | `compile_script(source_code)` | **No auth required, saves nothing** — use this to lint a draft before creating anything |
| `clone_script(id, name)` | `get_script(id)` → `add_script(new_name, source, "Lua")` | **There is no clone_script on Cloud.** This two-call pattern is the backup mechanism |
| `run_backtest(...)` / `run_backtest_and_wait(...)` | `execute_backtest(backtest_id, script_id, settings, start_unix, end_unix)` then poll `get_backtest_status` | No blocking variant exists; you must poll |
| `compare_backtests([a, b])` | `get_backtest_info` / `get_backtest_runtime` on each, compare fields yourself | **No compare tool on Cloud.** Record the metrics you care about explicitly |
| *(reading exported rows off disk)* | `get_backtest_export_data(backtest_id, name)` | Cloud gives you no filesystem access to the trade server |

Full Cloud tool set, for reference: `health_check`, `search_haasscript_docs`,
`list_scripts` / `get_script` / `add_script` / `edit_script_source` /
`compile_script` / `delete_script` / `is_script_executing`, `list_accounts` /
`list_markets` / `list_wallets` / `get_wallet` / `get_balance` /
`get_portfolio`, `check_market_data` / `sync_market_data`, `execute_backtest` /
`get_backtest_status` / `get_backtest_info` / `get_backtest_runtime` /
`get_backtest_logs` / `get_backtest_positions` / `get_backtest_export_data` /
`get_backtest_history` / `list_active_backtests` / `cancel_backtest` /
`delete_backtest`, `create_lab` / `clone_lab` / `update_lab` /
`get_lab_details` / `start_lab_execution` / `get_lab_execution_status` /
`get_lab_results` / `cancel_lab_execution` / `delete_lab` / `list_labs`,
`list_bots` / `get_bot` / `get_bot_runtime` / `get_bot_logs` /
`get_bot_positions` / `get_bot_open_orders` / `get_bot_profits` /
`get_bot_report`.

**Live bots are read-only over MCP.** You can observe state, positions, orders,
logs and profits. You cannot create, modify, start, stop or delete a bot — that
happens in the HTS web interface. Say so when asked, rather than attempting it.

### 0.3 Session opener

Start every session with `health_check`. Re-run it if tools begin returning
unexpected errors. Then resolve identifiers with `list_accounts` /
`list_markets` — never ask the user to paste a GUID, and never hardcode one into
a script or a saved settings block.

---

## 1. Cloud execution limits — read before planning any test

### 1.1 The caps

| Limit | Value | Error text on violation | Confidence |
|---|---|---|---|
| **Lab execution window** | **exactly 31 days** (2,678,400 s), **every tier** | `"Backtest period cannot exceed 1 month."` | *verified* — 31 d accepted, 31 d + 1 h rejected; the check is hard-coded and never reads the licence |
| **Lab parameter grid** | **1,000 combinations** | `"Too many possibilities. Please reduce the parameter range."` | multiply your ranges before launching |
| **Queued lab runs** | **5 per user** | `"Cannot queue more than 5 runs at the time."` | wait for one to finish or cancel it |
| **Standalone backtest window** | tier-dependent, see §1.2 | `"License limit"` | *verified* on Professional |
| **Window length vs lookback** | the cap is on **length only** | — | *verified* — a 100-day window starting 500 days back, and a 31-day window five years back, both ran |
| **Historical data depth** | years (BTC/USDT verified back to Jan 2018) | empty / partial result | confirm per market with `check_market_data` |
| **Concurrency** | **one running backtest per script**, not global | `"Cannot execute multiple backtests of the same script at once"` | *verified* — different scripts run in parallel with separate service IDs |
| **Rejected calls** | instant, start nothing, cost nothing | — | *verified* |

The concurrency rule has a useful corollary: **to parallelise a sweep of one
strategy, clone the script under a second name** (`get_script` → `add_script`)
and run the two windows against the two copies.

### 1.2 Standalone window by tier

A "month" is implemented as **31 days**.

| Tier | Published backtesting depth | Window cap | Confidence |
|---|---|---|---|
| Starter | 1 month | 31 d | no MCP access anyway |
| Standard | 6 months | **186 d** | *confirmed* |
| Professional | 12 months | **372 d** | *verified* — 372 d accepted, 373 d rejected |
| TradeServer | 36 months | 1,116 d | *confirmed*, not Cloud |

The server tests `totalDepthMinutes <= MaxBacktestMonths × 31 × 1440`, which is
where the 31-day month comes from. A trial is provisioned at Standard's 6 months.

### 1.3 Probe the tier instead of asking (free, instant)

Rejections cost nothing and start nothing, so determine the cap empirically
rather than interrogating the user about their subscription:

```
1. Pick any compiled script and a market with data.
2. execute_backtest over a 372-day window (12 × 31 d).
   → accepted  → Professional. Cap = 372 d. cancel_backtest immediately.
   → "License limit" → try 186 d (6 × 31 d).
       → accepted → Standard. Cap = 186 d. cancel_backtest immediately.
3. Labs are 31 days on both. No probe needed.
```

Cancel any run you started purely to probe. Record the discovered cap and reuse
it for the rest of the session rather than re-probing.

### 1.4 Designing a test plan that fits

The caps shape the workflow, not just the parameters:

- **Sweep in Labs over deliberately chosen 31-day regime months** — a bull
  month, a chop month, a bear month — rather than one long window. Rank by
  *worst-case* regime ROI, never by the sum (summing lets one great regime hide
  a catastrophic one).
- **Validate the survivors with one standalone backtest at the plan cap**
  (186 d or 372 d). This is the out-of-sample check the Labs window cannot give
  you.
- **For horizons beyond the cap**, chain consecutive standalone windows and
  stitch the equity curves — and state explicitly that compounding across
  stitches is approximate, because each window restarts from the configured
  balance.
- **Clone the configured lab (`clone_lab`) once per regime month** rather than
  rebuilding the parameter grid. The clone keeps the parameter list, so only the
  `start_lab_execution` dates change.

### 1.5 Payload budgets (MCP transport, not the server)

Cloud responses can exceed what an MCP client will ingest in one message. These
are *client* limits; the server returns complete data.

| Call | Observed size | Rule |
|---|---|---|
| `get_lab_results` | ~1.1 KB per entry (81 entries ≈ 95 KB) | keep `page_length` ≤ 45, paginate with `next_page_id` |
| `list_scripts` / `list_accounts` | 100–500 KB on mature accounts | filter and summarise, never dump raw |
| `get_backtest_export_data` | 31 d of per-minute rows = 44,641 rows ≈ 3.45 MiB *(verified complete, no server truncation)* | `Round(x, 2)` on every float cuts payload ~44%; budget in **minutes in period**, not chart bars |
| `get_backtest_positions` under `lab_id` | clamps `page_length` to 1000 | export data does **not** clamp on the lab path |

---

## 2. Phase 1 — Clarify intent before writing code

**Always draft an implementation plan and get user approval before touching any
script.** Extract answers from conversation history first; ask only what is
still missing.

| Question | Why it matters |
|---|---|
| Spot or futures/margin? | Determines position commands, leverage handling, and whether shorting is even available. A spot account cannot short — a long/short strategy on spot silently becomes long-only |
| Managed or unmanaged trading? | Managed = HaasScript places and tracks orders; unmanaged = you do |
| Entry logic — what triggers a trade? | Indicators, price levels, session times, ICT concepts |
| Exit logic — TP / SL / trailing / time? | Never skip. Missing exits cause runaway positions |
| Will parameters be swept in Labs? | Every `Input()` boolean must use `1`/`0` defaults, not `true`/`false` |
| New script or modify existing? | Existing → back up first (§7); never overwrite a working version |
| Which 31-day regime months will we sweep? | Decide *before* building, so the lab is cloned once per month |
| What is the validation horizon? | Must fit the plan cap (§1.2). If the user wants 2 years, say now that it needs stitched windows |
| What custom metrics should the report show? | Plan `CustomReport()` calls upfront |
| Will this be tuned later? | If yes, instrument entries/exits now — see **haasscript-export-instrumentation-cloud**. It is additive and makes tuning evidence-driven |

Present the plan as a numbered list of logic blocks. Proceed only after explicit
approval.

**Verify every command before you use it.** Call `search_haasscript_docs` to
confirm the exact signature, parameters and return type of any command you are
not 100 % certain about. It covers ~780 built-ins plus the user's own custom
commands, and it is the only reliable answer to "does this exist on *this*
server?" Guessing a signature is the single most common source of wasted cycles.

---

## 3. Phase 2 — Code structure template

```lua
-- =============================================================
-- StrategyName-vX.Y.Z-descriptor
-- Brief: one-line description of what this bot does
-- =============================================================

-- ── INPUT PARAMETERS ─────────────────────────────────────────
local myLength   = Input("Lookback Length", 20)
local myBool     = Input("Enable Feature", 1)   -- ALWAYS 1/0, never true/false
local mySelect   = Input("Mode", 0, "0=Fast,1=Safe")

-- ── INDICATOR CALCULATIONS ───────────────────────────────────
local closes     = ClosePrices()
local rsi        = RSI(closes, myLength)

-- ── ENTRY CONDITIONS ─────────────────────────────────────────
local enterLong  = rsi < 30
local enterShort = rsi > 70

-- ── POSITION STATE GUARD ─────────────────────────────────────
local pos        = GetPositionDirection()
local noPosition = pos == NoPosition
local isLong     = pos == PositionLong
local isShort    = pos == PositionShort

-- ── TRADE EXECUTION ──────────────────────────────────────────
if noPosition and not IsAnyOrderOpen() then
    if enterLong then
        DoLong("RSI Oversold")
    elseif enterShort then
        DoShort("RSI Overbought")
    end
elseif isLong and enterShort then
    DoExitPosition("Exit Long")
elseif isShort and enterLong then
    DoExitPosition("Exit Short")
end

-- ── PERSISTENCE / METRICS ────────────────────────────────────
-- Save() / Load() for state tracking goes here

-- ── FINALIZE (custom report) ─────────────────────────────────
Finalize(function()
    local fees     = Load('totalFees', 0)
    local gross    = Load('grossProfit', 0)
    CustomReport('Fee Drag Ratio', fees / Max(gross, 0.001))
end)
```

---

## 4. Phase 3 — Pre-compilation lint checklist

Run every item before calling `compile_script` / `edit_script_source`. These are
the most common causes of silent failure.

### 🔴 Critical — will break or crash

- [ ] **Boolean `Input()` defaults use `1`/`0`**, not `true`/`false`.
      Labs inject integer `1`; a boolean default produces
      `Cannot parse '1' to a boolean value`.
- [ ] **Array indexing starts at `1`**, never `0`. `closes[1]` is the most
      recent bar, `closes[2]` one bar back.
- [ ] **`Time()` for the current Unix timestamp.** Confirm any time helper with
      `search_haasscript_docs` — some names circulating in older guides
      (e.g. `CurrentUnix()`) are not registered.
- [ ] **Every `Load()` key has a matching `Save()`** on every execution path.
      A mismatched key silently returns the default every tick.
- [ ] **No `math.*` calls.** Use the HaasScript builtins: `Abs()`, `Round()`,
      `Max()`, `Min()`, `Floor()`, `Ceil()`.
- [ ] **Every command verified via `search_haasscript_docs`.** Command
      availability varies by HTS build — a name that works in one guide may be
      unregistered on this server. Verify, don't assume.

### 🟡 Important — causes subtle bugs

- [ ] **`interval` set explicitly in the settings block** (minutes). Omitting it
      inherits a default that is often 60 — a 1-minute strategy silently becomes
      a 1-hour one.
- [ ] **Leverage set explicitly on futures accounts.** Some exchange accounts
      default to very high leverage (125× is common on perpetuals); at that
      multiplier a small base-denominated size can fall under the venue's
      minimum notional and produce **zero trades with no error**. Pass
      `leverage: 1` unless the strategy genuinely wants leverage.
- [ ] **Trade size above the venue minimum.** Minimums are per-exchange and
      per-market — check the market's own limits via `list_markets` and confirm
      with a short probe backtest that positions actually open. A size below the
      minimum produces zero trades silently.
- [ ] **`scriptParameters` overrides in the settings block are unreliable.**
      HTS ignores keys it doesn't recognise, without error. The dependable way
      to change a parameter is to recompile with updated `Input()` defaults, or
      to sweep it in a lab.
- [ ] **`Plot()` records only from recompile forward** — it has no historical
      backfill. Use `PlotHorizontalLine(0, name, colour, value)` for persistent
      price levels.
- [ ] **`IfNull` works only with non-local (global) variables.** With a `local`
      it silently fails.
- [ ] **`DefineIntervalOptimization()` is valid only in module/container
      scripts** — it is a compile error in a standalone trade bot.
- [ ] **`Log()` calls concise and conditional.** Verbose unconditional logging
      floods backtest logs and buries the real signal.

### 🟢 Best practice

- [ ] `HideOrderSettings()` / `HideTradeAmountSettings()` when the strategy
      sizes its own orders (grids, unmanaged).
- [ ] `CustomReport()` inside `Finalize()` for fee drag, re-anchor count, grid
      utilisation, and any strategy-specific metric.
- [ ] `EnableHighSpeedUpdates()` only if sub-bar precision is genuinely needed.
- [ ] `OptimizedForInterval()` alongside technical-analysis commands — the
      compiler itself suggests it, and it measurably speeds up backtests. That
      matters more on Cloud than anywhere else, because a full-factorial lab
      multiplies the saving by the combo count.
- [ ] Version tag in the script name: `StrategyName-vX.Y.Z-descriptor`.
- [ ] Supported plot colours only: `"Aqua"`, `"Black"`, `"Blue"`, `"Cyan"`,
      `"DarkGray"`, `"DarkGreen"`, `"Fuchsia"`, `"Gold"`, `"Gray"`, `"Green"`,
      `"Maroon"`, `"Olive"`, `"Orange"`, `"Purple"`, `"Red"`, `"SkyBlue"`,
      `"Teal"`, `"White"`, `"Yellow"`.

---

## 5. Phase 4 — Mandatory pre-flight (blocking)

**No backtest fires until all five pass.** This section exists because a
misconfigured Cloud backtest reports `S=3` at 100 % completion while having
executed nothing at all.

### 5.1 `orderTemplate: 500` is mandatory

*Verified July 2026.* Omitting it makes **every managed trade action fail**
while the backtest still reports success:

```
ERROR: DoLong(): Execution error occurred.
ERROR: Argument out of range exception.
```

Misleading symptoms: `get_backtest_positions` returns `[]`, and any export file
contains rows right up to the first decision tick and then stops dead.
Reproduced on a bare `DoLong()` with no arguments, on both simulated and real
accounts, at several trade amounts. It is the settings block — not the script,
not the balance, not the account.

Known-good shape:

```json
{"botId":"","botName":"","accountId":"<uuid>","marketTag":"<tag>",
 "positionMode":0,"marginMode":0,"leverage":0,"tradeAmount":0.01,
 "interval":60,"chartStyle":300,"orderTemplate":500,"scriptParameters":{}}
```

**Better than hand-assembling it:** call `get_lab_details` on any lab that has
completed successfully and copy its `ST` block, swapping only account and
market. Labs populate `orderTemplate` themselves — which is exactly why lab runs
succeed while hand-rolled standalone runs of the same script fail.

`tradeAmount` is **base-denominated** (0.01 on a BTC market = 0.01 BTC, not $0.01).

### 5.2 The other four gates

| Gate | Call | Fail condition |
|---|---|---|
| **Account resolved** | `list_accounts` | never hardcode a GUID; they differ per deployment |
| **Market tag validated** | `list_markets` | tag must match the account type — a spot tag on a futures account (or vice versa) changes `GetPositionDirection()` behaviour |
| **Market data present** | `check_market_data(market, start, end, interval)` | empty/partial → `sync_market_data(market, start, end)` and retry. No auth needed for the check |
| **Window within cap** | arithmetic | ≤ 31 d for labs; ≤ the probed tier cap (§1.2) for standalone |

### 5.3 A failed run is indistinguishable from a good one at status level

Always, after every backtest:

```
get_backtest_status(backtest_id)      → S=3 means finished, NOT successful
get_backtest_logs(backtest_id)        → read for ERROR lines
get_backtest_positions(backtest_id)   → sanity-check the count is non-zero
```

Never report results from a run whose logs you have not read.

---

## 6. Phase 5 — Compile → debug cycle

### Step 1: syntax check without saving

```
compile_script(source_code)
```

No auth required, saves nothing. Returns `isValid`, `errors`, a `log` array, and
— most usefully — `inputs`: every registered `Input()` with its resolved `type`,
hash-prefixed `key` (e.g. `2-2-10-15.Enable Feature`) and `defaultValue` as a
string. Use it on every draft before creating a script record.

Two things to read off that response every time:

- **`type` on each input.** A boolean default renders as a non-`Number` type;
  anything a lab will sweep must come back as `Number` with a `defaultValue` of
  `"1"` or `"0"`. This is the cheapest possible catch for the #1 lab failure.
- **The `key` values.** These are exactly the hash-prefixed keys `update_lab`
  expects in its `parameters` array, so you can build the sweep grid before the
  script even exists as a record.

It also reports capability flags worth checking against intent: `isSpotSupported`,
`isMarginSupported`, `isLeverageSupported`, `isManagedTrading`, `isOneDirection`,
`isMultiMarket`.

### Step 2: create or update

```
add_script(name, script, type="Lua")                       -- new
edit_script_source(script_id, source_code, settings)       -- existing, keeps history
```

`edit_script_source` requires a settings object with at minimum `accountId`,
`marketTag`, `interval`, `tradeAmount`, `chartStyle`, `orderTemplate`.

### Step 3: confirm it saved and compiled

```
get_script(script_id)   -- check the compile result / validity field
```

If invalid, read the compiler errors before changing anything else.

### Compile error → fix table

| Error message | Root cause | Fix |
|---|---|---|
| `Cannot parse '1' to a boolean value` | `Input()` boolean default is `true`/`false` | change to `1`/`0` |
| `attempt to index a nil value` | array access on nil — wrong index or uninitialised source | index from 1; verify the data source returned something |
| `attempt to perform arithmetic on a nil value` | maths on a nil indicator result | guard: `if myVal ~= nil then …` |
| `'DefineIntervalOptimization' is not valid here` | used in a standalone bot | remove it; it belongs in modules |
| `unknown identifier '<name>'` | command not registered on this HTS build | `search_haasscript_docs` for the correct name |
| `unknown identifier 'math'` | Lua stdlib blocked | use `Abs()`, `Round()`, `Max()`, `Min()`, `Floor()`, `Ceil()` |
| compiles but takes no trades | configuration, not code | §7 |

### Step 4: regression test after any modification

There is **no `compare_backtests` on Cloud** — do it explicitly:

1. Note the backtest ID and key metrics of the last known-good run.
2. Re-run the **exact same** window, account, `interval`, `tradeAmount`,
   `leverage`, `positionMode`, `marginMode`.
3. Pull `get_backtest_info` / `get_backtest_runtime` on both and compare ROI,
   trade count, profit factor, fee total yourself.

**Acceptable variance:** ROI ± 0.5 %, trade count ± 2, profit factor ± 0.05.
Beyond that, the modification changed behaviour — find out why before moving on.

---

## 7. Phase 6 — Debugging zero-trade backtests

The most common scenario. Work the tree in order; roughly 80 % of cases resolve
in Tier 1.

### Tier 0: the Cloud-specific one — check this first

**Is `orderTemplate: 500` in the settings block?** (§5.1) If the logs show
`DoLong(): Execution error occurred` / `Argument out of range exception`, that
is the whole answer. Fix the settings, re-run, stop debugging the script.

### Tier 1: data and configuration

```
get_backtest_logs(backtest_id)
```

| Log content | Meaning | Fix |
|---|---|---|
| `Argument out of range exception` after a trade call | settings block missing `orderTemplate` | §5.1 |
| `"is syncing"` | market data not ready | wait, retry |
| `"Out of Sync"` | dynamic base-price reset fired | review re-anchor logic |
| `"Backtest has failed"` | execution crash | inspect `get_backtest_runtime` |
| price-data warnings | history gap | `sync_market_data(tag, start, end)` |

```
get_backtest_runtime(backtest_id)
```

Verify: `input_fields` (did the parameters actually apply? `scriptParameters`
overrides are unreliable — recompile with new defaults instead),
`positionMode` / `marginMode` correct for the account type, and
`open_orders` / `failed_orders` (orders attempted but rejected).

```
check_market_data(market_tag, start_unix, end_unix, interval)
```

### Tier 2: sizing and account type

- **Trade size** at or above the venue minimum, and remember it is
  base-denominated.
- **Leverage** — an unexpectedly high account default shrinks effective
  notional. Pass `leverage: 1` and re-run as a control.
- **Interval** — was it set explicitly, in minutes?
- **Spot account, short-side strategy** — spot cannot short. A long/short script
  on spot quietly becomes long-only and its behaviour will not match intent.
- **Wallet balance** — `get_wallet` / `get_balance`; a simulated account with no
  quote balance takes no trades.

### Tier 3: script logic

- **Condition never true** — add temporary `Log()` calls inside the entry block
  and read them back with `get_backtest_logs`.
- **Position guard never clears** — `not IsAnyOrderOpen()` stays false if an
  order is stuck; check `open_orders` in the runtime.
- **Signal container threshold** — `IndicatorContainer` unanimous (`[2]`) across
  three indicators fires very rarely; majority (`[3]`) is usually what was
  intended.

```lua
Log("RSI: " .. tostring(rsi), Orange)
Log("Position: " .. tostring(GetPositionDirection()), Blue)
Log("Any order open: " .. tostring(IsAnyOrderOpen()), Red)
```

Remove diagnostics before production — they slow backtests and flood logs.

---

## 8. Phase 7 — Version control without `clone_script`

Mandatory before any breaking change.

```
1. get_script(script_id)                         -- read current source
2. add_script("Name-vX.Y.Z-backup", source, "Lua")  -- named snapshot
3. edit_script_source(script_id, new_code, settings) -- modify in place,
                                                     -- preserves backtest history
4. execute_backtest(...)  → poll get_backtest_status → regression compare (§6 step 4)
5. Revert if needed: get_script(backup_id) → edit_script_source(original_id, backup_source, settings)
```

**Naming:** `StrategyName-vX.Y.Z-descriptor` — X = new trading approach or
breaking logic change, Y = new feature or input, Z = bug fix or default change;
descriptor is a short tag (`trend-filter`, `fee-fix`, `sl-tighten`, `bool-fix`).

Backup copies accumulate. `delete_script` the ones you no longer need, and be
aware that `list_scripts` on a mature account is a large payload (§1.5).

---

## 9. Phase 8 — Sweeping in Labs within the 31-day cap

```
create_lab(script_id, name, account_id, market, interval, style)
update_lab(lab_id, name, type, config, settings, parameters)
start_lab_execution(lab_id, start_unix, end_unix)        -- ≤ 31 days
get_lab_execution_status(lab_id)                          -- poll until S=3
get_lab_results(lab_id, next_page_id, page_length ≤ 45)
```

Mechanics that bite:

- **`create_lab` defaults `tradeAmount` to 100.0.** Override it in `update_lab`
  settings or the results are not comparable with your standalone runs.
- **Lab settings blocks include `orderTemplate` automatically** — which is why
  labs trade fine while hand-rolled standalone runs of the same script don't.
  Copy a working lab's `ST` block for your standalone settings.
- **Parameter values:** `O` is an **explicit list** (`[10, 30, 10]` sweeps those
  three values, it is *not* a progression). `range: [start, end, step]` is the
  shorthand that server-side expands into an inclusive explicit list. Use one or
  the other per entry, never both.
- **Algorithm type `0` with explicit `O` lists runs a full factorial.** The `SB`
  field in the start response tells you the combo count — check it before
  walking away.
- **Boolean inputs must be `1`/`0`** (repeated because it is the #1 lab failure).
- **Zero-trade configs silently win an ROI-sorted ranking.** `get_lab_results`
  sorts by ROI descending; a config that never trades scores 0.00, which
  outranks every losing config. **Filter on position count > 0 before ranking
  anything.**
- **Rank by worst-case regime ROI**, not summed or best — and tabulate
  positions-per-regime. If a directional strategy takes most of its trades in
  the regime that opposes its direction, that is a structural mismatch no
  parameter sweep can repair. Say so rather than shipping the least-bad
  parameter set as a win.
- **Drill into any combo in one call:** pass `lab_id` together with a result
  entry's `BID` to `get_backtest_info`, `get_backtest_runtime`,
  `get_backtest_logs`, `get_backtest_positions`, or `get_backtest_export_data`.
  No re-run as a standalone backtest is needed.

---

## 10. Phase 9 — Common patterns

### Safe indicator access

```lua
local closes = ClosePrices()
local rsi    = RSI(closes, 14)
if rsi == nil then return end          -- indicator still warming up
if rsi < 30 then DoLong("Oversold") end
```

### Offset — exclude the live bar

```lua
local high20 = GetHigh(HighPrices(), 20, 1)   -- offset 1 = exclude current bar
if ClosePrices() > high20 then DoLong("Breakout") end
```

### Signal consensus

```lua
local signals      = IndicatorContainer(EasyRSI(14, 30, 70), EasyMACD(12, 26, 9), EasyEMA(20, 50))
local unanimous    = signals[2]
local majorityVote = signals[3]
DoSignal(majorityVote)
```

### Persistent state

```lua
local fees  = Load('totalFees', 0)
local count = Load('tradeCount', 0)
if tradeFired then
    Save('totalFees',  fees + CurrentFee())
    Save('tradeCount', count + 1)
end
```

### Session filter (times are UTC)

```lua
local hour = CurrentHour()
if not (hour >= 13 and hour < 16) then return end   -- NY open window only
```

### Managed stop / target

```lua
local slPct = Input("Stop Loss %", 2.0)
local tpPct = Input("Take Profit %", 4.0)
SetStopLoss(slPct)
SetTakeProfit(tpPct)
if enterLong then DoLong("Entry") end
```

Scale stop distances with the chart interval — a 2 % stop that is sensible on
1 h is noise-triggered on 1 m. And be careful with **armed trailing stops**: a
trailing variant that only activates after a profit threshold leaves losing
trades entirely unprotected. Pair it with a hard stop.

### Unmanaged sizing (grids)

```lua
HideOrderSettings()
HideTradeAmountSettings()
-- place and track your own orders here
```

### Custom report

```lua
Finalize(function()
    local g, f = Load('grossProfit', 0), Load('totalFees', 0)
    CustomReport('Gross Profit',   g)
    CustomReport('Total Fees',     f)
    CustomReport('Fee Drag Ratio', f / Max(g, 0.001))
end)
```

Fee drag ratio is the fastest structural diagnostic there is. It is **invariant
to trade size** — doubling the position doubles both gross and fees — so a
strategy with a ratio above ~1.0 is not fixable by trading bigger. Compare
cost-per-round-trip against edge-per-trade instead, and if the edge is smaller,
the strategy needs fewer/better trades, not more size.

---

## 11. Command quick reference

Always confirm a signature with `search_haasscript_docs` before use. This list
is a memory aid, not an authority — availability varies by HTS build.

```lua
-- Price data
ClosePrices()  OpenPrices()  HighPrices()  LowPrices()  BidPrices()  AskPrices()

-- Raw indicators (return arrays)
RSI(src, length)   EMA(src, length)   SMA(src, length)
MACD(src, fast, slow, signal)          -- {macd, signal, hist}
BollingerBands(src, length, mult)      -- {upper, mid, lower}
IchimokuCloud(tenkan, kijun, senkou)   -- {tenkan, kijun, spanA, spanB}

-- Easy indicators (return SignalLong / SignalShort / SignalNone)
EasyRSI(length, oversold, overbought)  EasyMACD(fast, slow, signal)
EasyEMA(fast, slow)                    EasyBollingerBands(length, mult)
EasyOBV()                              EasyMFI()

-- Positions
GetPositionDirection()   -- PositionLong | PositionShort | NoPosition
GetPositionAmount()      IsAnyOrderOpen()
DoLong(note)   DoShort(note)   DoExitPosition(note)   DoSignal(sig)   DoFlipPosition(note)

-- Extremes with offset
GetHigh(HighPrices(), length, offset)   GetLow(LowPrices(), length, offset)

-- Time / persistence / wallet
CurrentHour()  CurrentMinute()  Time()
Save("key", value)   Load("key", default)
WalletAmount(AccountGuid(), "USDT")

-- Output
Log("msg")   Plot(0, "Label", value, Orange)   PlotHorizontalLine(0, "Label", Green, price)
CustomReport("name", value)

-- Control
DeactivateBot("reason")   EnableHighSpeedUpdates()
HideOrderSettings()       HideTradeAmountSettings()
```

**Easy Safeties take one `if` each.** Chaining several safety checks into a
single compound condition changes their semantics — give each its own guard.

---

## 12. Gotcha reference

| Gotcha | Detail | How to verify on your server |
|---|---|---|
| Settings missing `orderTemplate: 500` | every managed trade action fails; run still reports 100 % complete | copy the `ST` block from a completed lab (`get_lab_details`) |
| `true`/`false` boolean inputs | labs inject `1`/`0`; parse error | `compile_script` returns the discovered input fields — inspect them |
| `scriptParameters` overrides ignored | HTS drops unrecognised keys silently | check `input_fields` in `get_backtest_runtime` after the run |
| `interval` not set → inherited default | 1-minute strategy runs at 1 h | set it explicitly in the settings block (minutes) |
| Command unregistered on this build | `unknown identifier` | `search_haasscript_docs` before writing |
| Array index 0 | runtime error | arrays start at 1, always |
| `math.*` blocked | `unknown identifier 'math'` | use `Abs`/`Round`/`Max`/`Min`/`Floor`/`Ceil` |
| `IfNull` with `local` | silently fails | use a global |
| `Plot()` has no history | empty chart before recompile | `PlotHorizontalLine()` for persistent levels |
| High default leverage on futures | zero trades, no error | pass `leverage: 1` and re-run as a control |
| Trade size below venue minimum | zero trades, no error | check `list_markets` limits; probe with a short backtest |
| Spot account, short logic | silently long-only | `list_accounts` → confirm account type before writing |
| One backtest per script | `"Cannot execute multiple backtests of the same script at once"` | clone the script (`get_script` → `add_script`) to parallelise |
| Lab window > 31 d | `"Backtest period cannot exceed 1 month."` | arithmetic before the call |
| Standalone window > tier cap | `"License limit"` | probe once per session (§1.3) |
| `create_lab` `tradeAmount` = 100.0 | results incomparable, no error | always set it in `update_lab` |
| Zero-trade configs top the lab leaderboard | ROI 0.00 beats every loss | filter position count > 0 before ranking |
| Oversized MCP payloads | truncation / client spill | page `get_lab_results` ≤ 45; `Round(x, 2)` in exports |
| Live bots unmanageable over MCP | read-only by design | direct the user to the HTS web interface |

---

## 13. Output format for a new script

When presenting a completed script, always include:

1. **Script name** following `StrategyName-vX.Y.Z-descriptor`.
2. **One-paragraph plain-English summary** of the logic.
3. **Input parameters table** — name, default, purpose, and whether it is
   intended for a lab sweep.
4. **Lint checklist result** — confirm the critical items passed.
5. **The code**, fenced.
6. **Pre-flight result** — resolved account, validated market tag, confirmed
   data coverage, and the settings block including `orderTemplate: 500`.
7. **Suggested test plan** — which 31-day regime months to sweep, and which
   standalone window (within the probed tier cap) to validate against.

After compiling, confirm validity via `get_script(script_id)` and state the
script ID clearly so it can be referenced in later calls. After backtesting,
**read the logs before reporting any number.**
