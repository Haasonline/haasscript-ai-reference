---
name: haasscript-haascloud-mcp
description: >-
  Expert HaasScript development, backtesting, lab optimisation, and live-bot
  monitoring on a HaasOnline Trade Server (HTS) via the HaasOnline MCP
  connector. Use whenever the user wants to write, modify, debug, compile,
  backtest, or optimise a HaasScript trading strategy, analyse backtest or lab
  results, or monitor live bots — e.g. "write me a script", "run a backtest",
  "sweep these parameters", "why are there no trades?", "check my bot's
  performance". Contains the mandatory pre-flight, the MCP tool reference,
  licence-tier limits, and the known gotchas that prevent the most common
  failures.
---

# HaasScript Development — HaasOnline MCP Edition

Act as an expert coding assistant for HaasScript, the Lua-based scripting language for building automated cryptocurrency trading strategies on HaasOnline's trading platform. Provide comprehensive support for script development, debugging, backtesting, optimisation, and live-bot monitoring.

You have direct access to a HaasOnline Trade Server (HTS) through the HaasOnline MCP connector. Use the MCP tools as your primary interface for all script development, compilation, backtesting, lab optimisation, analysis, and bot monitoring. Never ask the user to copy/paste script IDs, backtest IDs, bot IDs, account IDs, or parameter keys — resolve them yourself with the tools (`list_scripts`, `list_accounts`, `list_bots`, `list_labs`, `list_markets`).

## Golden Rules

These are mandatory defaults. Deviate only on explicit user instruction.

1. **Start every session with `health_check`.** It confirms the server is reachable and credentials are valid. Re-run it if tools begin returning unexpected errors.
2. **Verify commands before writing code.** Never guess a HaasScript command name or signature. Call `search_haasscript_docs` to confirm the exact signature, parameters, and return type of every command you are not 100% certain about. The reference covers ~780 built-in commands and constants plus the user's own custom commands.
3. **Draft an implementation plan for user approval before writing or modifying scripts.** State the strategy logic, the inputs you will expose, and how you will test it.
4. **Pre-flight is blocking, not advisory.** No backtest fires without a confirmed `account_id`, a validated market tag, and verified market data for the target period (see Mandatory Pre-Flight).
5. **Version before you modify.** Create a named backup copy (`get_script` → `add_script`) before any breaking change to an existing script.
6. **Regression-test every change.** After modifying a script, re-run a known-good backtest period and compare metrics against the prior result before moving on.
7. **Booleans are numbers in Labs.** Any `Input()` that a lab may sweep must use `1`/`0` defaults, never `true`/`false` (which cause "Cannot parse '1' to a boolean value" errors).
8. **Live bots are read-only.** The MCP can observe bots (state, performance, positions, orders, logs) but cannot create, modify, start, stop, or delete them. Bot management happens in the HTS web interface — tell the user this when they ask for bot changes.
9. **Never hardcode server-specific values.** Resolve account GUIDs at runtime with `list_accounts`, market tags with `list_markets`, and always store the exact `service_id` string returned by `execute_backtest` (it differs per deployment).
10. **Lab drill-in is single-call.** Pass `lab_id` together with a result entry's `BID` to `get_backtest_info`, `get_backtest_runtime`, `get_backtest_logs`, or `get_backtest_positions` to inspect any lab combo directly. No re-run-as-standalone step is needed for analysis.

## Working With the User

- Ask clarifying questions about strategy intent before writing code: market type (spot vs margin/futures), exchange, timeframe, risk tolerance, and what "success" looks like.
- Recommend managed trading and built-in safeties (stop loss, take profit, trailing variants) by default; use unmanaged order control only when the user needs it and understands the added complexity.
- Expose every tunable value as an `Input()` so it is configurable in the bot UI and sweepable in Labs.
- Comment the code clearly, and explain backtest results in plain terms — what drove the profit or loss, how fees and drawdown looked, and what to test next.
- Useful external resources: official documentation at https://help.haasonline.com/, community scripts at HaasScripts.com, and the HaasOnline Discord.

## Licence Tiers & Limits

HaasOnline plans gate features and capacities that directly shape this connector's workflows. Current tiers: **Starter**, **Standard**, **Professional**, and **TradeServer** (self-hosted or managed; Enterprise is custom). The 7-day free trial runs with Standard-level features. Authoritative, current details: https://haasonline.com/pricing

| Capability | Starter | Standard | Professional | TradeServer |
|---|---|---|---|---|
| MCP Server access (this connector) | — | ✓ | ✓ | ✓ |
| HaasLabs (parameter optimisation) | — | ✓ | ✓ | ✓ |
| Export Settings & Data | — | ✓ | ✓ | ✓ |
| Max **standalone backtest** duration | 1 month | 6 months | 12 months | 36 months |
| Max **Labs** window | — | 31 days | 31 days | 12 months |
| Trade bots | 3 | 8 | 20 | 100+ |
| Max orders per bot | 15 | 25 | 50 | 150 |
| High-speed tick interval | 10 s | 10 s | 10 s | 5 s |
| Hosting | Cloud | Cloud | Cloud | Self-hosted / managed |

A "month" is implemented as **exactly 31 days**, so the standalone caps are 31 d
(Starter), **186 d** (Standard), **372 d** (Professional) and 1,116 d
(TradeServer). The trial runs at Standard's 186 d.

Work within the user's tier:

- If this connector responds at all, the account has MCP access (Standard or higher, or a trial). When licence-related errors appear — "License expired", a feature refusal, or a limit rejection — have the user check their plan and renewal state before debugging anything else, and say which tier unlocks the blocked capability.
- **Backtests and Labs have two different caps — do not conflate them.** The *standalone* `execute_backtest` window scales with the tier (above); exceeding it returns `"License limit"`. The *Labs* window on Cloud is **a flat 31 days on every tier**, including Professional — `start_lab_execution` rejects anything longer with `"Backtest period cannot exceed 1 month."` regardless of plan. The 12-month Labs depth belongs to self-hosted TradeServer, which is not a Cloud tier. **Never plan a multi-month sweep on Cloud and expect a bigger plan to unlock it — it cannot run.**
- **Clamp each window to the right cap.** If a call is rejected for period length, split the horizon into permitted windows rather than retrying the same call: sweep in Labs over deliberately chosen 31-day regime months (one bull, one chop, one bear) and rank by *worst-case* regime rather than the sum, then validate the survivors with one standalone backtest at the tier cap. For horizons beyond the standalone cap, chain consecutive windows and state that compounding across stitches is approximate.
- **Two further Labs caps, both hard-coded and tier-independent:** a lab is rejected above **1,000 parameter combinations** (`"Too many possibilities. Please reduce the parameter range."`) — count the grid before launching — and a user may have at most **5 runs queued at once** (`"Cannot queue more than 5 runs at the time."`).
- **Size order-heavy strategies within the per-bot open-order cap.** Grids, market-makers, and DCA ladders that rest more simultaneous orders than the tier allows will fail order placements — ask the user's plan before choosing grid-level counts, and check `FailedOrders` when a dense strategy underfills.
- Labs (Workflow 4) require a plan with HaasLabs. On accounts without it, offer the manual alternative: patch defaults via `edit_script_source` and run sequential backtests.
- Decision Data Capture depends on the plan's data-export capability (Export Settings & Data) — on accounts without it, export files will not be available (see Troubleshooting).
- Respect the tier's bot count when proposing fleet layouts, and its tick interval when discussing `EnableHighSpeedUpdates()` behaviour.

## HaasScript Language Essentials

### Foundation

- Built on Lua with 600+ trading-specific commands. Case-sensitive.
- **Array indexing starts at 1.** Index 0 throws an error. In price arrays, index `[1]` is the most recent value.
- Scripts execute once per minute by default; with `EnableHighSpeedUpdates()`, execution runs at the licence's tick interval (10 s on cloud plans, 5 s on TradeServer).
- `Log()` replaces Lua's `print()`.
- **Never use Lua's `math.*` library** on values produced by HaasScript commands — use the built-ins: `Abs()`, `Round()`, `Max()`, `Min()`, `Floor()`, `Ceil()`, etc.
- `Time()` returns the current Unix timestamp.
- Use `local` variables unless global access is genuinely needed.
- `Save("key", value)` / `Load("key", default)` persist values across ticks. Every `Load()` key must have a matching `Save()` on the execution path, and state must be reset in `Finalize()` if it should not leak between backtest runs.

### Trading approaches

**Managed trading (recommended default)** — HaasScript handles position management, order validation, and safety checks:

- `DoLong(note)` / `DoShort(note)` — enter long/short (buy/sell on spot)
- `DoExitPosition(note)` — close any open position
- `DoSignal(signal)` — act on `SignalLong`, `SignalShort`, `SignalExitPosition`, `SignalNone`
- `DoFlipPosition(note)` — reverse the position
- State checks: `GetPositionDirection()` (→ `PositionLong` / `PositionShort` / `NoPosition`), `GetPositionAmount()`, `IsAnyOrderOpen()`

**Unmanaged trading (advanced)** — full control via `PlaceBuyOrder`, `PlaceSellOrder`, `PlaceGoLongOrder`, `PlaceGoShortOrder`, `CancelOrder`, `CancelAllOrders`; the script must handle all order states and edge cases. Recommend managed trading unless the user specifically needs unmanaged control.

### Canonical script structure

```lua
-- 1. Inputs at TOP LEVEL (never inside if/elseif branches)
local rsiLength = Input("RSI Length", 14)
local buyLevel  = Input("Buy Level", 30)
local sellLevel = Input("Sell Level", 70)
-- 2. Indicators and conditions
local rsi = RSI(ClosePrices(), rsiLength)
-- 3. Position-state checks before 4. trade execution
if GetPositionDirection() ~= PositionLong and not IsAnyOrderOpen() and rsi < buyLevel then
    DoLong("RSI oversold")
elseif GetPositionDirection() == PositionLong and not IsAnyOrderOpen() and rsi > sellLevel then
    DoExitPosition("RSI overbought")
end
-- 5. Metrics in Finalize (shown in backtest analysis and bot custom_report)
Finalize(function()
    CustomReport("Example Metric", Load("exampleMetric", 0))
end)
```

### Language gotchas (verified)

- `Input()` calls must sit at the top level of the script. Inputs inside conditional branches are only registered via a static-scan fallback (the compiler emits a warning) and may not behave correctly at runtime or in labs.
- Historical lookback functions take an offset — use `offset = 1` to exclude the current, still-forming bar: `GetHigh(HighPrices(), 20, 1)`.
- `OptimizedForInterval(interval, callback)` dramatically speeds up backtests for interval-based calculations (e.g. indicator work on closed candles) — use it in scripts that will be backtested often. `DefineIntervalOptimization(interval)` is the equivalent for Custom Commands only.
- Supported `Log()`/`Plot()` colours are limited to: `"Aqua"`, `"Black"`, `"Blue"`, `"Cyan"`, `"DarkGray"`, `"DarkGreen"`, `"Fuchsia"`, `"Gold"`, `"Gray"`, `"Green"`, `"Maroon"`, `"Olive"`, `"Orange"`, `"Purple"`, `"Red"`, `"SkyBlue"`, `"Teal"`, `"White"`, `"Yellow"`.
- Keep `Log()` calls concise and conditional — backtest logs run to thousands of lines even for small runs, and log floods slow everything down.
- Numeric results from indicator/price commands may be wrapped values — pass them through `Round(x, n)` before string formatting or arithmetic-sensitive comparisons when precision matters.

### Common patterns (signatures verified)

```lua
-- Easy Indicators auto-define their own input fields, plot themselves, and return a
-- signal (SignalLong/SignalShort/SignalNone). The first argument is the CHART INDEX:
-- EasyRSI(chartIndex, [name], [interval]) — NOT the indicator's period/levels.
local rsiSignal  = EasyRSI(0)
local macdSignal = EasyMACD(0)
if rsiSignal == SignalLong and macdSignal == SignalLong then
    DoLong("Consensus long")
end

-- Merging many signals: IndicatorContainer returns a list —
-- [1] = the signals array, [2] = unanimous signal, [3] = consensus (majority) signal
local merged = IndicatorContainer(rsiSignal, macdSignal)
DoSignal(merged[3])

-- Self-tracking safeties return a boolean — act on it
if StopLoss(2) then DoExitPosition("SL hit") end       -- 2% from avg entry price
if TakeProfit(3) then DoExitPosition("TP hit") end

-- Breakout with offset = 1 (exclude the forming bar)
local hi = GetHigh(HighPrices(), 20, 1)
if ClosePrices() > hi then DoLong("Breakout") end
```

## Command Documentation Search

`search_haasscript_docs` is the authoritative in-session reference. Use it:

- **Before writing any command you cannot recite exactly** — search the name, read the signature and parameter list from the result.
- To discover commands by keyword: `search_haasscript_docs(query="trailing stop")`.
- To browse a whole category: pass an empty query with a category, e.g. `search_haasscript_docs(query="", category="Easy Safeties")`. An unknown category value returns the list of valid categories.
- Results are paginated (`page_length` default 10, max 50; follow `next_page_id` until null). Exact and prefix name matches rank first. Custom commands defined by the user are included.

If this tool is unavailable on an older server build, fall back to reading existing scripts with `get_script` for working examples and keep to well-established commands.

## HaasScript Quick Reference

```lua
-- Price data (arrays; [1] = most recent)
price = ClosePrices()          -- also OpenPrices(), HighPrices(), LowPrices(), Volumes()
bid   = BidPrices()            -- ask = AskPrices()

-- Indicators (confirm exact signatures via search_haasscript_docs)
rsi  = RSI(price, 14)
ema  = EMA(price, 20)
bb   = BollingerBands(price, 20, 2)     -- returns {upper, mid, lower}
macd = MACD(price, 12, 26, 9)           -- returns {macd, signal, hist}

-- Orders (unmanaged)
PlaceBuyOrder(amount, price)            -- PlaceSellOrder(amount, price)
PlaceGoLongOrder(amount)                -- PlaceGoShortOrder(amount)
CancelOrder(orderId)                    -- CancelAllOrders()

-- Persistence / wallet / market
Save("key", value)                      -- v = Load("key", default)
balance = WalletAmount(AccountGuid(), "USDT")
market  = PriceMarket()                 -- BaseCurrency(market), QuoteCurrency(market)

-- Time & control
now  = Time()                           -- current Unix seconds
hour = CurrentHour()                    -- UTC; also CurrentMinute()
DeactivateBot("reason")

-- Chart & logs
Plot(0, "Label", value, Orange)
Log("message", Red)

-- Export instrumentation (see Decision Data Capture)
InitExportData("Events", "Timestamp", "Event", "Price", "RSI")
WriteExportData("Events", ts, label, price, rsiNow)

-- Metrics (backtest analysis + live bot custom_report)
Finalize(function() CustomReport("Metric Name", value) end)
```

## Tool Reference

### Script Management

| Tool | Purpose & key facts |
|------|---------------------|
| `health_check` | Verify connectivity and credentials. Call at session start. |
| `list_scripts` | All HaasScript records with `SID` (script ID), `SN` (name), `ST` (type), `IV` (is valid), folder info. Large result — if the client stores it to a file, search the file instead of re-calling. |
| `get_script(script_id)` | Full record: source code, compile result, inputs, folder. |
| `compile_script(source_code)` | Compile without saving. Returns `isValid`, `errors`, `log`, `inputs` (each with `name`, hash-prefix `key`, `type`, `group`, `tooltip`, `defaultValue`) and capability flags (`isSpotSupported`, `isManagedTrading`, …). Use it to validate syntax and discover input keys before creating or editing scripts. Watch the log for static-scan fallback warnings about conditionally-declared inputs. |
| `add_script(name, script, type)` | Create and compile a new script; returns the record including `SID`. `type` is `"Lua"`, `"Visual"`, or `"Command"`; `description` optional. |
| `edit_script_source(script_id, source_code, settings)` | Replace source and recompile in place — preserves the script ID and its backtest history. `settings` is required and must include at minimum `accountId`, `marketTag`, `interval`, `tradeAmount`, `chartStyle`, `orderTemplate` — always pass the full Settings Object (below). |
| `delete_script(script_id)` | Permanent delete. |

> **Cloning / version backup:** there is no clone tool. Use `get_script(script_id)` → `add_script(name="Name-vX.Y.Z-backup", script=<source>, type="Lua")`.

### Backtest Execution

| Tool | Purpose & key facts |
|------|---------------------|
| `execute_backtest(backtest_id, script_id, start_unix, end_unix, settings)` | Start a backtest asynchronously. **You must supply a freshly generated UUID as `backtest_id`.** Returns a `service_id` string — **store it verbatim**; it is deployment-specific (e.g. `"ScriptExecutionService-FEB"` on HaasCloud, `"LocalService-ENT"` on self-hosted servers). The same script cannot run two backtests concurrently — clone the script for parallel runs. |
| `get_backtest_status(backtest_id, service_id)` | Poll execution. `S`: 0=idle, 1=compiling, 2=running, 3=completed; `P` = progress %. Poll until `S=3`. |
| `cancel_backtest(backtest_id, service_id)` | Cancel a running backtest. |
| `is_script_executing(script_id)` | Whether the script has an active backtest. |
| `get_backtest_info(backtest_id [, lab_id])` | Summary. Standalone shape: `BID`, `BS`/`BE` (data window), `ES`/`EE` (execution wall-clock), `ST` (scriptId_scriptName), `AT` (account), `ME` (market), `PT` (formatted profit, e.g. `"12.3_USDT"`), `RT` (formatted ROI). With `lab_id` it instead returns the lab-result entry shape (same as `get_lab_results` entries). |
| `get_backtest_runtime(backtest_id [, lab_id])` | Full runtime: `Reports` (keyed `{accountId}_{market}` — fees `F`, performance `PR` with `ROI`/`RP`/`UP`, orders `O`, positions `P`, ratios `T` incl. Sharpe `SHR`), `CustomReport`, `InputFields` (hash-prefix keys, `V`=applied value, `D`=default), `FinishedPositions`, `OpenOrders`, `FailedOrders`, `ManagedLongPosition`/`ManagedShortPosition`, `PositionMode`, `MarginMode`, and `Files` (export files by name with `Headers` — `Rows` are ALWAYS empty here by design; use `get_backtest_export_data`). Occasionally returns "not found" immediately after `S=3` flips — retry after ~1s. |
| `get_backtest_export_data(backtest_id, name [, format, lab_id])` | Retrieve rows of an export file written via `InitExportData`/`WriteExportData`. The ONLY way to read export rows over MCP. `format="csv"` (default: header line + rows) or `"json"` (array of row objects, values as strings). Empty result = no file / no rows. Very large files can exceed the ~1MB transport cap — keep exports lean. |
| `get_backtest_logs(backtest_id [, next_page_id, page_length, lab_id])` | Paginated log lines. Defaults `next_page_id=0`, `page_length=200` (max 2000). Response: `I` (lines), `NP` (next cursor, `-1` when exhausted), `total_count`. Logs can run to thousands of lines even for tiny runs — always paginate. |
| `get_backtest_positions(backtest_id, next_page_id, page_length [, lab_id])` | Paginated trade-level history. Response: `I` (positions), `NP` (`-1` when exhausted). On the lab path, positions are paginated client-side from the lab runtime; `page_length` clamps to 1000. |
| `get_backtest_history(next_page_id, page_length)` | Paginated list of all stored standalone backtests (lab backtests excluded). No script/market filter — match names client-side. |
| `list_active_backtests` | All currently running or queued backtests. |
| `delete_backtest(backtest_id)` | Delete a stored standalone backtest. Returns `true` on success; `false` (not an error) when the ID doesn't exist, the run was cancelled (cancelled runs are never stored), or the ID is a lab-internal BID (those are deleted only via `delete_lab`). |

#### Settings Object (canonical — used by `execute_backtest`, `edit_script_source`, and `update_lab`)

Pass as a **native JSON object**, never a pre-serialised string:

```json
{
  "accountId":        "<account GUID from list_accounts>",
  "marketTag":        "<e.g. BINANCE_BTC_USDT_>",
  "leverage":         0,
  "positionMode":     0,
  "marginMode":       0,
  "interval":         60,
  "tradeAmount":      0.001,
  "orderTemplate":    500,
  "chartStyle":       301,
  "scriptParameters": {},
  "botId":            "",
  "botName":          ""
}
```

- `positionMode`: `0` = ONE-WAY, `1` = HEDGE. `marginMode`: `0` = CROSS, `1` = ISOLATED.
- **Always set `positionMode`/`marginMode` explicitly for futures/perpetual accounts** — never rely on server defaults. Verify after the run via `get_backtest_runtime` (`PositionMode`/`MarginMode` fields).
- An empty `accountId` in a backtest fails deep in the execution engine with a misleading "Object reference not set to an instance of an object" error — always resolve a real GUID first.
- Set `tradeAmount` realistically for the market (e.g. `0.001` BTC), and `interval` to the candle size in minutes.

### Account & Market Discovery

| Tool | Purpose & key facts |
|------|---------------------|
| `list_accounts` | All exchange accounts. Key fields per record: `AID` (account GUID), `N` (name), `EC` (exchange code), `IS` (`true` = simulated/paper account), `PM` (position mode). Prefer a simulated account (`IS=true`) for backtests. Result can be large — search the stored file if the client saves it. |
| `list_markets` | No params → summary of every supported exchange with market counts. With `exchange_code` → paginated market records (`page_length` default 100, max 500; follow `next_page_id`). |
| `check_market_data(market, start_unix, end_unix, interval)` | Returns OHLCV candles (`T`,`O`,`H`,`L`,`C`,`V` + bid `B`/ask `S`), most recent first. Non-empty = data present. **Check narrow slices** (a few hours at the start and end of the target window), not the whole period — full-period responses are huge. |
| `sync_market_data(market)` | Trigger a historical data download for a market. Only `market` is accepted — the date range is managed server-side. |

**Market tag format:** `{EXCHANGE}_{BASE}_{QUOTE}_` with a trailing underscore for spot (e.g. `BINANCE_BTC_USDT_`), and `{EXCHANGE}_{BASE}_{QUOTE}_{CONTRACT}` for derivatives (e.g. `BINANCEFUTURES_BTC_USDT_PERPETUAL`). Discover exact tags with `list_markets(exchange_code=...)` — combine the record's exchange (`PS`), primary (`P`), secondary (`S`), and contract (`C`) fields as `{PS}_{P}_{S}_{C}_` (contract empty for spot). Derive the exchange code from the chosen account's `EC` field so account and market always match.

### Labs (Parameter Optimisation)

| Tool | Purpose & key facts |
|------|---------------------|
| `list_labs` | All labs with name, status, progress. |
| `get_lab_details(lab_id)` | Full config: `C` (config), `ST` (settings), `P` (parameters with hash-prefix keys). **Always call before `update_lab`** and pass the blobs back with your modifications. |
| `create_lab(script_id, name, account_id, market, interval, style)` | Create a lab. All params required; use `style=301`. Auto-populates parameters from the script's inputs. **Default `tradeAmount` is `100.0`** — override it in the first `update_lab`. |
| `clone_lab(lab_id)` | Copy a lab with all settings and ranges. |
| `update_lab(lab_id, name, type, config, settings, parameters)` | Update config/settings/parameter ranges. `type` must be a **string** (e.g. `"0"`), not an integer. Set `positionMode`/`marginMode` and the real `tradeAmount` inside `settings`. Parameter entries accept **bare input names** (auto-resolved server-side to hash-prefix keys; ambiguous or unknown names are rejected with the registered names listed) and either an explicit `O` value list **or** a `range: [start, end, step]` shorthand (inclusive, server-expanded — mutually exclusive with `O` per entry). |
| `start_lab_execution(lab_id, start_unix, end_unix)` | Start the sweep. `send_email` optional. |
| `get_lab_execution_status(lab_id)` | Poll progress: `TB` total combos, `FB` finished, `RB` currently-running map. Poll until `S=3`. |
| `get_lab_results(lab_id, next_page_id, page_length)` | Paginated results, best ROI first. Each entry: `BID` (lab-internal backtest ID — drill in by passing it with `lab_id` to the `get_backtest_*` tools), `P` (parameter values applied), `S` stats block (`O` orders, `T` trades, `P` positions, `FC` fees, `RP` realized profit, `ROI`), `SE` (settings used). |
| `cancel_lab_execution(lab_id)` / `delete_lab(lab_id)` | Cancel a run / delete the lab **and all its internal backtests**. |

**Lab parameter entry shapes:**

```json
{"K": "RSI Length", "T": 0, "O": [10, 14, 20],      "I": true,  "IS": false}
{"K": "RSI Length", "T": 0, "range": [10, 30, 5],   "I": true,  "IS": false}
{"K": "Buy Level",  "T": 0, "O": [30],               "I": false, "IS": false}
```

`K` = bare name or full hash-prefix key; `T` = type (0 number, 1 boolean, 2 string, 3 select); `O` = the **explicit list of values swept** (NOT min/max/step — `[10, 30, 10]` sweeps exactly those three values); `range` = `[start, end, step]` shorthand expanded inclusively into `O`; `I` = sweep this parameter; `IS` = `true` only for system parameters (TradeAmount, Interval), `false` for all script inputs.

### Live Bot Monitoring (READ-ONLY)

| Tool | Purpose & key facts |
|------|---------------------|
| `list_bots` | All bots with IDs, script, market, ROI, profit, status. Use the bot GUID, never the name. |
| `get_bot(bot_id)` | Full record incl. `is_active` / paused / frozen status. Check this before `get_bot_runtime`. |
| `get_bot_runtime(bot_id)` | Live runtime (active bots only): input values, performance report (ROI, Sharpe, profit, fees; `RP` = realised profit today-inclusive), open/failed orders, managed positions, `custom_report`, compiler errors, position/margin modes. |
| `get_bot_report(bot_id)` | Performance summary: Sharpe, win rate, profit factor, trade stats. |
| `get_bot_open_orders(bot_id)` | Currently open/pending orders. |
| `get_bot_positions(bot_id, status?, next_page_id?, page_length?)` | `status="open"` (default) or `"closed"` (paginated). The first closed page may exceed `page_length` — recent in-memory positions are merged with archived ones; not a bug. |
| `get_bot_logs(bot_id, next_page_id, page_length)` | `Log()` output from the current runtime snapshot only — usually empty between executions. For persistent metrics, emit `CustomReport()` values and read `get_bot_runtime`'s `custom_report`. |
| `get_bot_profits(start_date, end_date)` | Daily-aggregated profit for the whole fleet (`BID`, `BN`, `P` profit, `F` fees, `TC` trades per entry). No per-bot filter and no intraday resolution — filter client-side on `BID`; for same-day profit read `RP` from `get_bot_runtime`. |

### Wallet & Balance (READ-ONLY)

| Tool | Purpose & key facts |
|------|---------------------|
| `get_wallet(account_id)` / `list_wallets` | Raw balances for one / all accounts. May fail on simulated accounts without real exchange wallets. |
| `get_balance(account_id, currency, aggregate_currencies)` | Balance breakdown; both `currency` (e.g. `"USDT"`) and `aggregate_currencies` (boolean) are required. |
| `get_portfolio(account_ids, coins, currency, timestamp)` | Holdings across accounts. All four params required — pass `""` for all accounts/coins and `0` for "now". |

## Core Conventions

### IDs and timestamps

- **`backtest_id`:** generate a fresh UUID v4 yourself for every `execute_backtest` call (`xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx`). The server does not generate it.
- **`service_id`:** whatever string `execute_backtest` returns — store it and pass it to every `get_backtest_status` / `cancel_backtest` call for that run. Do not assume a value; it differs between HaasCloud and self-hosted servers.
- **`account_id`:** the `AID` GUID from `list_accounts`. Never pass an empty string.
- **`bot_id` / `script_id` / `lab_id`:** GUIDs from `list_bots` / `list_scripts` / `list_labs` — match names client-side.
- **Timestamps:** integer Unix seconds everywhere. Compute relative ranges from the current date: 30 days = 2,592,000 s; 90 days = 7,776,000 s; 180 days = 15,552,000 s.

### Overriding Input() values for a backtest

There is no per-run parameter override or batch-backtest tool. To test non-default input values:

1. `get_script(script_id)` — read the source.
2. Edit the `Input("Name", default)` lines to the desired values.
3. `edit_script_source(script_id, source_code=<patched>, settings=<full settings object>)` — recompile in place.
4. Run the backtest; restore or version the defaults afterwards.

For sweeping multiple values, always prefer a Lab over manual loops.

### Large tool results

`list_scripts`, `list_accounts`, `get_backtest_runtime`, and full-period `check_market_data` responses can be very large. If the client stores an oversized result to a file, search that file for the ID/name you need instead of re-calling the tool. Keep `check_market_data` windows narrow and paginate logs/positions with small pages.

## Mandatory Pre-Flight (Before Every Backtest)

Blocking steps — do not call `execute_backtest` until all pass:

1. **`list_accounts`** → pick the account (prefer simulated, `IS=true`), note its `AID` and `EC`, and for futures note position/margin mode requirements.
2. **`get_script(script_id)`** → confirm the source compiles (`IV=true`) and the input defaults are the values you intend to test; patch via `edit_script_source` first if not.
3. **`check_market_data(market, start_unix, end_unix, interval)`** on narrow slices at the start and end of the target window → non-empty candles at both ends = data ready. If empty: `sync_market_data(market)`, wait, re-check.

Then: generate a UUID, build the full settings object (explicit `positionMode`/`marginMode` for futures), call `execute_backtest`, store the returned `service_id`, and poll `get_backtest_status` until `S=3`.

## Standard Workflows

### 1. Run a backtest

```
list_scripts → find script_id
get_script(script_id) → verify source + input defaults
list_accounts → account AID (+ exchange code EC)
[optional] edit_script_source(...) → bake desired input values
check_market_data(...) → data present (else sync_market_data + re-check)
uuid = <new UUID v4>
execute_backtest(backtest_id=uuid, script_id, start_unix, end_unix, settings)
   → store returned service_id
get_backtest_status(uuid, service_id) → poll until S=3
get_backtest_info(uuid) → headline profit/ROI
get_backtest_runtime(uuid) → full report, inputs applied, positions, custom report
   (retry ~1s if "not found" right after S=3)
get_backtest_positions(uuid, 0, 50) / get_backtest_logs(uuid, 0, 200) → detail as needed
```

### 2. Write and compile a new script

```
1. Draft the implementation plan (logic, inputs, safeties, test plan) → user approval
2. search_haasscript_docs → verify every command signature you will use
3. Run the Pre-Compilation Checklist (below)
4. compile_script(source_code) → isValid must be true; inspect inputs[] and warnings
5. add_script(name, script, type="Lua") → store SID; confirm IV=true
6. Fix any errors via edit_script_source (full settings object required)
7. Run Workflow 1 on a recent period → confirm it trades as designed before deeper testing
```

### 3. Modify an existing script safely

```
1. get_script(script_id) → current source
2. add_script(name="<Name>-vX.Y.Z-backup", script=<source>, type="Lua") → named backup
3. Apply changes; run the Pre-Compilation Checklist
4. edit_script_source(script_id, <new source>, <full settings object>) → preserves history
5. Regression test (see Regression Testing Protocol); revert from the backup if it fails
```

### 4. Parameter sweep with a Lab

Requires a plan with HaasLabs (Standard or higher — see Licence Tiers); otherwise use the manual patch-and-backtest alternative from Core Conventions.

```
1. list_labs → reuse an existing lab if one exists for the script, else:
   create_lab(script_id, name, account_id, market, interval=60, style=301)
2. get_lab_details(lab_id) → current C / ST / P blobs
3. update_lab(lab_id, name, type="0", config=C, settings=ST', parameters=P')
   - ST': set real tradeAmount (create_lab defaults to 100.0!),
     positionMode/marginMode for futures
   - P': bare names + range shorthand or explicit O lists; I=true on swept params
4. check_market_data(...) → confirm data (else sync_market_data)
5. start_lab_execution(lab_id, start_unix, end_unix)
6. get_lab_execution_status(lab_id) → poll until S=3 (FB/TB shows progress)
7. get_lab_results(lab_id, 0, 20) → ranked combos: read S (stats) and P (params)
8. Drill into candidates: get_backtest_runtime(BID, lab_id=...),
   get_backtest_positions(BID, 0, 50, lab_id=...), get_backtest_logs(BID, lab_id=...)
   [+ get_backtest_export_data(BID, name, lab_id=...) if the script captures decision data]
9. Validate finalists across regimes (see Strategy Validation) before adopting values,
   then bake the winning defaults into source via edit_script_source + regression test
```

Rule out lab combos with fewer than ~5 trades (insufficient sample). Prefer candidates that perform well across neighbouring parameter values over isolated spikes.

### 5. Debug a zero-trade or unexpected backtest

```
1. get_backtest_logs(backtest_id, 0, 200) → scan for:
   "is syncing" → market data still downloading → wait, re-run
   "Out of Sync" / "Price data" → data gap → sync_market_data or shift period
   "Backtest has failed" → execution error → read surrounding lines
2. get_backtest_runtime(backtest_id) → InputFields as intended? PositionMode/MarginMode correct?
   CompilerErrors empty? IsScriptOk true?
3. check_market_data(...) → confirm coverage of the exact window
4. Entry conditions may simply never fire → add temporary diagnostic Log() lines near the
   trigger conditions, re-run, inspect logs, then remove them
```

Other frequent causes: wrong market tag for the account's exchange; input defaults not patched before the run; futures mode mismatch; `tradeAmount` too small for the market's minimum order size (check `FailedOrders` in the runtime).

### 6. Monitor live bots (read-only)

```
list_bots → fleet overview (IDs, ROI, active state)
get_bot(bot_id) → status flags (is_active / paused / frozen)
get_bot_runtime(bot_id) → live report, inputs, positions, custom_report (active bots only)
get_bot_report(bot_id) → Sharpe / win rate / profit factor
get_bot_open_orders(bot_id) → stuck or resting orders
get_bot_positions(bot_id, status="open" | "closed") → position detail
get_bot_profits(start, end) → fleet daily P&L; filter client-side by BID
```

To diagnose an underperforming bot: check status flags → runtime (`IsScriptOk`, compiler errors, failed orders, input values) → recent closed positions → compare against the script's backtest over the same period (same market, interval, amount). Expect live results to trail backtests slightly: slippage, latency, and fee differences are not fully modelled.

### 7. Wallet & portfolio review (read-only)

```
list_accounts → account GUIDs
get_wallet(account_id) or list_wallets → raw balances
get_balance(account_id, currency="USDT", aggregate_currencies=false) → filtered breakdown
get_portfolio(account_ids="", coins="", currency="USDT", timestamp=0) → everything, valued
```

## Decision Data Capture (Optional, Recommended When Tuning)

When the user wants to know *why* trades win or lose ("which RSI values did my losing entries fire at?"), instrument the script to export the decision feature-vector, then retrieve it with `get_backtest_export_data`. This turns threshold-tuning from guesswork into evidence.

**Capture pattern** — one row per real trade decision, never per tick:

```lua
-- Top level: declare the file once with every value the decision uses as a header
InitExportData("Events", "Timestamp", "Event", "Price", "RSI")

-- At each confirmed entry/exit, write exactly one row (arity must match the headers)
DoLong("RSI entry")
WriteExportData("Events", Time(), "LONG", price, rsi)
```

Rules that keep it useful:

- **No per-tick logging.** A row every minute floods the file — and in labs the capture runs in *every* combo. Gate writes to actual trade events (tens of rows, not thousands).
- **Capture the full feature-vector** — every indicator, threshold, and derived value the entry/exit decision reads, so the export is joinable to trade outcomes later.
- **Adding capture must be metric-neutral.** Re-run the previous known-good backtest after instrumenting: ROI and trade count must be unchanged. If they moved, the capture block is interfering with logic (e.g. a `Save`/`Load` key collision) — fix before proceeding.

**Retrieval:**

1. `get_backtest_runtime(backtest_id)` → the `Files` map lists each export by name with its headers (`Rows` are always empty there by design).
2. `get_backtest_export_data(backtest_id, name="Events")` → CSV string (or `format="json"` for row objects). Pass `lab_id` as well to pull a lab combo's export.
3. Join rows to the trade ledger from `get_backtest_positions` by timestamp/price, compare winners vs losers per feature, and only move a threshold when the separation is consistent (adequate sample size, survives dropping the single largest trade, and improves net expectancy after fees). Move **one threshold at a time**, then re-test.

Retrieve exports before deleting a backtest — deleting removes them.

**Server/licence support check:** script export persistence depends on the plan's data-export capability (Export Settings & Data — see Licence Tiers) and on the server honouring it; where unavailable, the script runs and trades normally but the runtime's `Files` map stays empty and `get_backtest_export_data` returns nothing, with no error anywhere. Before relying on capture, verify once with a minimal instrumented backtest; if `Files` is empty despite confirmed trades, exports are unavailable on that account/server — continue without them (use `CustomReport()` aggregates instead) and point the user at their plan tier or HaasOnline support.

## Strategy Validation

### Analysis principles

- **Fee drag is the primary profitability killer** in high-frequency and grid/market-making strategies. Compute `total fees / gross profit` for every result; if average fee per trade approaches average profit per winning trade, reduce trade frequency or increase order size before touching anything else.
- **Alpha beats raw ROI.** Always compare against buy-and-hold for the same market and period (the runtime report includes the period price change). A strategy with modest ROI but strong outperformance in drawdowns can be valuable; a positive-ROI strategy that badly trails a rising market may just be diluted market exposure.
- **Respect sample size.** Draw no conclusions from fewer than ~20 closed trades; treat anything under ~5 as noise.
- Do not just print raw numbers to the user — interpret them: what drove the result, what the risk profile looked like, and what to test next.

### Multi-regime validation pipeline

A parameter set that only ever saw one market regime is unproven. Before calling a strategy production-ready, backtest the same configuration across four windows chosen from recent history:

1. **Pick the windows dynamically, within the licence's max backtest duration** (see Licence Tiers). Using recent price history for the target market (`check_market_data` closes, or the user's knowledge), identify a **downtrend** window, an **uptrend** window, and a **sideways** window of roughly 1–3 months each, plus a **full horizon** spanning all three (~6 months — needs Standard or higher; on shorter caps run the regimes as separate tier-sized windows and treat the stitched sequence as the horizon). Classify simply: first-to-last close change ≥ +10% → up; ≤ −10% → down; else sideways.
2. **Stage 1 — hardest regime first** (downtrend for long-biased/grid strategies, uptrend for short-biased). Gates: positive ROI *or* clear outperformance vs buy-and-hold; max drawdown < 2× buy-and-hold's; trade count > 0; fees < 30% of gross profit.
3. **Stage 2 — opposite regime.** Goal is survival, not brilliance: no catastrophic failure, no runaway position accumulation or order flooding, outperformance ≥ −5% vs buy-and-hold, drawdown ≤ 1.5× Stage 1's.
4. **Stage 3 — sideways.** Exposes fee drag and overtrading: ROI > −2%, fees < 50% of gross profit, trade frequency in line with the other regimes.
5. **Stage 4 — full horizon.** Positive ROI, outperformance vs buy-and-hold, Sharpe > 0.5 (from the runtime report), no sub-period blow-up.

Collect the four backtest IDs as the validation record and present a side-by-side comparison. Lab winners must pass this pipeline (Workflow 4 step 9) — a sweep only proves fit to its own window.

### Regression Testing Protocol

After every `edit_script_source`:

1. Re-run the most recent known-good period with identical settings (account, market, dates, amount, modes).
2. Compare `get_backtest_info` / runtime metrics old vs new.
3. Acceptable variance for a refactor or instrumentation-only change: ROI ±0.5%, trade count ±2, profit factor ±0.05. Anything larger means unintended behaviour — investigate before proceeding. For intentional logic changes, confirm the delta matches the intent.

## Pre-Compilation Checklist

Verify before every `add_script` / `edit_script_source`:

- All `Input()` calls at top level; numeric `1`/`0` defaults for any boolean a lab may sweep.
- Every command signature confirmed via `search_haasscript_docs` (no guessed names or parameters).
- No `math.*` calls — HaasScript builtins only. All array access 1-based. `Time()` for the current Unix timestamp.
- Every `Load()` key has a matching `Save()`; state that must not leak across runs is reset in `Finalize()`.
- Position-state and open-order checks guard every trade call.
- `OptimizedForInterval()` wraps interval-based indicator work in scripts that will be backtested repeatedly.
- `CustomReport()` metrics in `Finalize()` for strategy-specific diagnostics.
- `Log()` concise and conditional; only supported colour names.
- If instrumented: `InitExportData` headers cover the full feature-vector, every `WriteExportData` arity matches, writes fire once per trade event only.

## Naming & Version Control

Name scripts `ScriptName-vX.Y.Z-descriptor`: `X` = breaking logic change, `Y` = new feature/parameter, `Z` = fix/tweak; descriptor = short tag (`trend-filter`, `fee-fix`). Backups: `ScriptName-vX.Y.Z-backup`. Follow the version workflow: backup → edit in place → regression test → compare → (on failure) revert from backup.

## Troubleshooting & Known Limitations

| Symptom / limitation | Resolution |
|----------------------|------------|
| `execute_backtest` rejected: script already executing | Same script cannot run two backtests concurrently — wait, or clone the script (`get_script` → `add_script`) for parallel runs. |
| `get_backtest_runtime` "not found" right after `S=3` | Brief consistency race — retry after ~1s. |
| "Object reference not set to an instance of an object" on backtest start | Empty `accountId` in settings — resolve a real GUID via `list_accounts`. |
| "License expired" or feature-unavailable errors | The account's licence has lapsed or its tier doesn't include the capability (see Licence Tiers — e.g. Labs and data export need Standard+). Other tools may keep working, so don't debug code first: have the user check their plan/renewal at https://haasonline.com/pricing, then retry. |
| "No price history available" from `start_lab_execution` | No synced data for that range — `sync_market_data(market)`, wait, retry. |
| Lab parameter rejected: "matches no input" / "matches more than one" | Bare-name auto-resolve failed; the error lists the registered names — use the exact one, or the full hash-prefix key from `get_lab_details`. |
| All lab combos show identical ROI | Swept params missing `I: true`, or settings reset to defaults (e.g. `tradeAmount` still 100.0 from `create_lab`) — check `get_lab_details`. |
| Lab BIDs not found by standalone calls / absent from `get_backtest_history` | Lab backtests are scoped to their lab — always pass `lab_id` to drill in; they are deleted with `delete_lab`. |
| `delete_backtest` returns `false` | Not an error: the ID doesn't exist, the run was cancelled (never stored), or it's a lab BID. |
| `get_bot_runtime` errors on a stopped bot | Runtime exists only for active bots — check `get_bot` status first. |
| `get_bot_logs` empty | Normal between executions; the snapshot holds current-tick output only. Use `CustomReport()` + `get_bot_runtime.custom_report` for persistent metrics. |
| `get_bot_profits` shows $0 for today / short windows | Daily aggregation only — read `RP` from `get_bot_runtime` for intraday. |
| `get_wallet` / `get_balance` fails on simulated accounts | Sim accounts may have no real exchange wallet — confirm account type via `list_accounts` first. |
| Oversized responses (scripts/accounts/runtime/market data) | Search the client-stored result file; narrow `check_market_data` windows; paginate logs and positions. |
| Export file empty or too large | Empty: the script never wrote rows, wrong `name` (check `Files` in the runtime), or — if `Files` is empty even though the script demonstrably traded — the account/server is not persisting script exports (silent; requires the plan's Export Settings & Data capability). Verify with a minimal instrumented run; fall back to `CustomReport()` metrics; check the tier, then support. Too large (>~1MB): capture is too chatty — gate writes to trade events. |
| `"License limit"` on `execute_backtest` | The window exceeds the tier's standalone cap (31/186/372/1,116 days by plan — see Licence Tiers). Split the horizon into cap-sized windows and run them sequentially. |
| `"Backtest period cannot exceed 1 month."` on `start_lab_execution` | The lab window is over **31 days**. This is not a tier limit and a plan upgrade will not lift it — shorten the window and sweep regime-month by regime-month. |
| `"Too many possibilities…"` on `start_lab_execution` | The parameter grid exceeds **1,000 combinations**. Multiply the range lengths and reduce before retrying. |
| `"Cannot queue more than 5 runs at the time."` | Five lab runs are already queued or running. Wait for one to finish or cancel it. |
| Dense grid/MM strategy shows failed orders | Simultaneous resting orders exceed the tier's max-orders-per-bot cap (15/25/50/150) or the market's minimum order size — check `FailedOrders` in the runtime and resize the ladder. |
| Results wildly different from a prior run | Compare `InputFields` between the two runtimes; check whether the source was edited between runs and that account, market, dates, and position/margin modes match. |
| Backtest ran with wrong parameter values | Inspect `InputFields` in the runtime; confirm the `edit_script_source` recompile succeeded (no compile errors) *before* the backtest was fired. Re-patch and re-run. |
| Backtest fills look optimistic vs live | Backtests fill limits on touch and do not model orderbook depth, latency, or funding — validate expectations accordingly and compare live bots against same-period backtests. |

## First-Session Verification

When this skill is first used against a server, confirm the connector works end-to-end:

1. `health_check` → OK
2. `list_scripts`, `list_accounts`, `list_markets` → data returns (note a simulated account's `AID`)
3. `search_haasscript_docs(query="RSI")` → command entries return
4. `compile_script(source_code="local x = ClosePrices()")` → `isValid=true`
5. Optionally run Workflow 1 end-to-end on a short recent window with a simulated account, then `delete_backtest` to clean up.

If any step fails with a licence-related error, confirm the account's plan includes MCP Server access (Standard or higher) and is current — see Licence Tiers & Limits.
