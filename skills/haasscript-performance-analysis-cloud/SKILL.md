---
name: haasscript-performance-analysis-cloud
description: >
  HaasScript strategy performance analysis workflow for HaasOnline Cloud via the
  HaasOnline MCP. Use whenever the user asks to analyse, evaluate, review, or
  interpret backtest results, lab results, or live bot performance on a Cloud
  server — "how did the backtest go?", "is this strategy good?", "what do the lab
  results mean?", "why is ROI negative?", or anything involving ROI, alpha,
  Sharpe, fee drag, drawdown, win rate, or profit factor. Includes the Cloud
  server's verified execution limits (31-day lab window, license-capped
  standalone window) and the multi-window workflow that works within them.
---

# HaasScript Performance Analysis — HaasOnline Cloud Edition

## Purpose

Transform raw HaasOnline backtest / lab / live bot data into clear, actionable
verdicts — while working inside the execution limits of the HaasOnline **Cloud**
server. Every analysis must produce: a **headline verdict**, a **primary
constraint diagnosis**, and at least one **concrete next action**.

This edition is written for any HaasOnline customer using the Cloud MCP
connector. It contains no server-specific IDs, dates, or account references —
resolve those at runtime with `list_scripts`, `list_accounts`, `list_markets`,
`list_labs`, `list_bots`.

---

## Cloud Server Limits (verified by live probing, July 2026)

These limits were confirmed empirically against a production HaasOnline Cloud
server. Where a limit is license-dependent this is noted — re-probe on the
user's own server (cheap: rejected calls error instantly and cost nothing)
rather than assuming.

| Limit | Value | Error on violation | Notes |
|---|---|---|---|
| **Lab execution window** | **exactly 31 days** (2,678,400 s) | `"Backtest period cannot exceed 1 month."` | 31d accepted; 31d + 1h rejected. Applies to `start_lab_execution` only — platform-wide, regardless of tier (verified on a Pro-tier server). |
| **Standalone backtest window** | **license-tier dependent** — see tier table below | `"License limit"` | Verified on a Pro server: 372d (= 12 × 31d) accepted; 373d rejected. A "month" is implemented as 31 days. |
| **Window vs lookback** | Limit is on **window length only** | — | A 100-day window starting 500 days in the past is accepted; a 31-day window 5+ years back is accepted. You can test any historical period, just not in one long slice. |
| **Historical data depth** | Years (BTC/USDT daily data verified back to Jan 2018) | empty/partial result | Confirm per market with `check_market_data` (no auth required) before relying on an old period. |
| **`get_lab_results` page size** | No server cap observed at 81 results, but ~1.1 KB per entry | oversized MCP payload | 81 entries ≈ 95 KB — larger than most MCP clients can ingest in one response. Keep `page_length` ≤ 45 and paginate via `next_page_id`. |
| **Large `list_*` payloads** | `list_scripts` / `list_accounts` on mature accounts return 100–500 KB | oversized MCP payload | Expect big responses to be truncated or spilled by the client. Filter/summarise rather than dumping raw output. |
| **New-lab `tradeAmount` default** | **100.0** | silent — no error | `create_lab` ignores your intended size until you set it via `update_lab` settings. Results are incomparable with standalone runs until fixed. |
| **Live bots** | Read-only over MCP | — | Observe state/positions/orders/logs only. Create/start/stop/delete happens in the HTS web interface. |
| **Rejected calls are free** | — | — | Both window errors return instantly without starting anything, so probing a server's boundary costs nothing. |

### Standalone backtest window by license tier

Per HaasOnline's published plan comparison (haasonline.com/pricing, July 2026),
maximum backtest history per tier:

| Tier | Published limit | Expected window cap (months = 31 days) | Notes |
|---|---|---|---|
| Starter | Up to 1 month | 31 days | HaasLabs **not included** on Starter |
| Standard | Up to 6 months | 186 days | a trial is provisioned at this tier |
| Pro | Up to 12 months | **372 days (verified by probing)** | |
| TradeServer | Up to 36 months | 1,116 days | not a Cloud tier |

The server tests `totalDepthMinutes <= MaxBacktestMonths × 31 × 1440`, which is
where the 31-day month comes from. On any given server you can still find the
boundary in two or three `execute_backtest` probes — a rejected call errors
instantly with `"License limit"` and starts nothing. Other tier limits that
shape analysis work: trade bots 3/8/20/100+, max orders per bot 15/25/50/150
(Starter/Standard/Pro/TradeServer).

**The Labs window does not scale with the tier.** It is a flat 31 days on every
Cloud plan including Pro — the check is hard-coded and never consults the
licence, so a bigger plan will not unlock a longer sweep. Two further Labs caps
behave the same way: a grid above **1,000 combinations** is rejected
(`"Too many possibilities. Please reduce the parameter range."`), and a user may
have at most **5 runs queued at once**
(`"Cannot queue more than 5 runs at the time."`).

**Practical throughput reference:** 3 concurrent labs × 81 combos each, 1-month
window at 1h interval, completed in ~13 minutes total. Use this to estimate
sweep runtimes before launching.

### How the limits reshape the workflow

The old "run one long backtest across every regime" habit does not survive the
Cloud caps. The Cloud-native pattern is:

1. **Sweep in Labs, one regime-month at a time.** Each lab run covers at most
   31 days, so pick *deliberately chosen months* — one bear, one bull, one
   sideways (see Regime Windows below).
2. **Clone, don't rebuild.** `clone_lab` preserves the full parameter grid;
   only the `start_lab_execution` dates change per regime month. Rebuilding
   the grid by hand for each month invites transcription errors.
3. **Validate survivors standalone.** `execute_backtest` allows up to the
   license cap (31 days on Starter through 36 months on TradeServer) in a
   single run — use it for the long-horizon confirmation of the top 3–5 lab
   configs.
4. **Chain windows for longer horizons.** When the wanted horizon exceeds the
   tier cap, run consecutive cap-length standalone backtests and stitch the
   equity curves. State clearly in the report that compounding across stitched
   windows is approximate.
5. **Starter tier has no Labs at all.** There, parameter comparison means
   scripted series of standalone 31-day backtests with varied
   `script_parameters` — slower, but the same regime-month methodology applies.

---

## Analysis Workflow

### Step 0 — Session Preconditions

- `health_check` first. Re-run it if tools start returning unexpected errors.
- Resolve every ID yourself (`list_scripts`, `list_accounts`, `list_labs`,
  `list_bots`, `list_markets`). Never ask the user to paste IDs.
- Before any backtest: confirmed `account_id`, validated market tag, and
  `check_market_data` for the target period. Pre-flight is blocking.
- Store the exact `service_id` string returned by `execute_backtest` — it is
  required by `get_backtest_status` / `cancel_backtest` and differs per
  deployment and per run.

### Step 1 — Collect the Data

| Need | Tool |
|---|---|
| Single backtest metrics | `get_backtest_analysis(backtest_id)` (or `get_backtest_info`) |
| Trade-level detail | `get_backtest_positions(backtest_id)` |
| Debug logs | `get_backtest_logs(backtest_id)` |
| Parameter verification | `get_backtest_runtime(backtest_id)` |
| Lab results ranked | `get_lab_results(lab_id, next_page_id=0, page_length=45)` |
| Lab combo deep-dive | pass `lab_id` **plus** the entry's `BID` to `get_backtest_info` / `get_backtest_runtime` / `get_backtest_logs` / `get_backtest_positions` |
| Live bot state | `get_bot_runtime(bot_id)` |
| Closed bot trades | `get_bot_positions(bot_id, status="closed", limit=20)` |

**Always call `get_backtest_runtime` when results look suspicious** — it
reveals the parameters actually used (overrides often silently revert to
compiled defaults).

**Payload discipline (Cloud-specific):** cap page sizes and limits on every
call (`page_length` ≤ 45, position/log `limit` ≤ 20–50) and paginate. A single
oversized response can be unreadable in the MCP client.

### Step 2 — Compute Derived Metrics

The server does not return these directly; calculate them from raw output.

#### Alpha vs Buy-and-Hold

```
alpha = strategy_ROI − hold_ROI_for_same_period
```

Compute `hold_ROI` from actual prices — never from memory:

```
candles = check_market_data(market, start, end, interval=1440)
hold_ROI = (close_at_end / close_at_start − 1) × 100
```

`check_market_data` requires no authentication and daily candles for a 1-month
window are a tiny payload. There is no excuse for a guessed benchmark.

> A strategy at −5% ROI while the market fell 30% has **+25% alpha** — strong.
> A strategy at +10% ROI while the market rose 50% has **−40% alpha** — weak.

#### Fee Drag Ratio

```
fee_drag = total_fees / gross_profit        (gross ≤ 0 → effectively ∞)
```

| Fee Drag | Verdict |
|---|---|
| < 0.20 | Healthy |
| 0.20–0.40 | Moderate — monitor |
| 0.40–0.70 | Heavy — reduce round trips or fee rate |
| > 0.70 | Catastrophic — the strategy works for the exchange |

**Fee drag is invariant to trade size.** Fees and gross P&L both scale
linearly with notional, so raising `tradeAmount` scales the loss, not the
ratio. Never recommend "trade larger" as a fee fix. The only real levers:
**fewer round trips**, **bigger average move per trade**, **lower fee rate**
(maker vs taker, fee tier).

#### Cost per Round Trip vs Edge per Trade

```
round_trip_cost ≈ 2 × fee_rate            (e.g. ≈ 0.2% of notional on typical spot)
avg_gross_per_trade = gross_profit / total_trades   (as % of notional)
```

If `avg_gross_per_trade < round_trip_cost`, the strategy loses to fees no
matter how it is sized. Print this ratio before touching any parameter — it
says immediately whether the problem is **cost** or **edge**.

#### Profit Factor

```
profit_factor = gross_profit / gross_loss
```

| PF | Verdict |
|---|---|
| < 1.0 | Losing |
| 1.0–1.3 | Marginal |
| 1.3–2.0 | Acceptable |
| > 2.0 | Strong |

### Step 3 — Regime Windows and Gates (Cloud method)

Hardcoded regime dates go stale and rarely match the user's market. Build the
windows fresh, sized to the 31-day lab cap:

**Selecting regime months**

1. Pull ~12–18 months of daily candles for the target market
   (`check_market_data`, interval 1440 — chunk the calls if the payload is
   large).
2. Compute each calendar month's return and high-low range.
3. Pick, from the most recent data available:
   - **Bear month** — most negative return (ideally ≤ −10%)
   - **Bull month** — most positive return (ideally ≥ +10%)
   - **Sideways month** — return closest to 0% with an ordinary range
4. Record each window's unix start/end (≤ 31 days each — a 31-day request is
   accepted, 31 days + 1 hour is not) and the market's own return for the
   window: that is the alpha benchmark.
5. **Full horizon** — one standalone backtest over the longest span the
   license allows (see tier table; probe the cap, shortening on
   `"License limit"` until accepted).

**Gates** (all four must PASS for production readiness):

| Stage | Window | Primary Gate | Secondary Gate |
|---|---|---|---|
| 1 — Bear month | lab, ≤31d | Alpha > 0% OR ROI > 0% | MaxDD < 2× market's own drawdown |
| 2 — Bull month | lab, ≤31d | Alpha ≥ −5% | No runaway positions |
| 3 — Sideways month | lab, ≤31d | ROI > −2% | Fee drag < 0.5 |
| 4 — Full horizon | standalone, ≤ license cap | ROI > 0% | Alpha > 0%, Sharpe > 0.5 |

**Bear is make-or-break for long-biased, grid, and market-making strategies.
Bull is make-or-break for short-biased or mean-reversion-short strategies.
Run the hostile regime first** — it disqualifies fastest.

Reporting format:

```
REGIME VALIDATION SUMMARY
──────────────────────────────────────────────────────
Stage 1 — Bear      [month]  ROI: X%  Alpha: Y%  MaxDD: Z%  → PASS/FAIL
Stage 2 — Bull      [month]  ROI: X%  Alpha: Y%  MaxDD: Z%  → PASS/FAIL
Stage 3 — Sideways  [month]  ROI: X%  Alpha: Y%  FeeDrag: Z → PASS/FAIL
Stage 4 — Full      [span]   ROI: X%  Alpha: Y%  Sharpe: Z  → PASS/FAIL
──────────────────────────────────────────────────────
Overall: PASS / CONDITIONAL / FAIL
Primary Constraint: [fee drag | signal quality | regime mismatch | drawdown]
```

### Step 4 — Diagnosis

Identify the **single primary constraint** and map it to the correct fix.

#### A) Zero Trades

Triage in order:
1. `get_backtest_logs` → look for `"is syncing"`, `"Out of Sync"`,
   `"Price data"`, `"Backtest has failed"`
2. `get_backtest_runtime` → parameters resolved? `position_mode` /
   `margin_mode` correct?
3. `check_market_data` → data actually present for the window?

| Root Cause | Fix |
|---|---|
| Market data not synced | `sync_market_data(tag, start, end)`, then retry |
| Spot tag used on futures account (or vice versa) | Use the matching tag type from `list_markets` |
| Trade size below exchange minimum | Increase trade amount |
| Futures leverage left at default | Pass `leverage` explicitly |
| `script_parameters` silently ignored | Recompile with hardcoded defaults; verify via runtime |
| Wrong interval defaulted | Pass `interval` explicitly |
| Lab boolean input given `true`/`false` | Use `1`/`0` — labs cannot parse booleans |

#### B) Zero-trade configs poison lab rankings (Cloud lab gotcha)

`get_lab_results` sorts by ROI descending. A config that never trades scores
0.00 — which **outranks every losing config**. In heavy sweeps, dozens of
zero-trade combos can occupy the entire top of the leaderboard.

**Filter on position count > 0 before ranking anything.** Then rank by
worst-regime ROI, not summed ROI — summing lets one good month hide a
catastrophic one.

#### C) Negative ROI + many trades → fee drag

Compute `fee_drag` and cost-vs-edge (Step 2). If fee drag > 0.5:

| Fix | Why it works |
|---|---|
| Widen entry/exit spread or thresholds | Fewer round trips, bigger move per trade |
| Fewer grid levels / longer interval | Fewer round trips (widen stops proportionally — see E) |
| Minimum-profit threshold per trade | Blocks trades that cannot cover the round trip |
| Maker orders / better fee tier | Directly lowers the fee rate |

*(Not on the list: bigger orders — size does not change the ratio.)*

**Warning:** cutting fees can unmask a missing edge. If fee drag falls but ROI
worsens, entries are the problem, not costs. Re-test the "fixed" config in an
opposite regime month before calling it an improvement — a single-period result
cannot distinguish a config change from a regime change.

#### D) Positive ROI + negative alpha → capital bleed

- Bleed only in the bull month → strategy misses upside; add a trend filter.
  Fixable.
- Bleed in all regimes → it's a capital-preservation hedge, not an alpha
  generator. Say so plainly; it may still have a place, but not as "the
  strategy that beats the market".

#### E) High drawdown

Check `get_backtest_positions`:
- One catastrophic loss → sizing or stop placement.
- Slow bleed across many trades → structural regime mismatch (see F).

Also: stop distances must scale with the candle interval. Moving 15m → 1h
without widening stops/trails proportionally lets normal candle noise stop out
nearly every trade. Treat interval and stop distance as one setting.

#### F) Structural regime mismatch — the trade-count diagnostic

Before tuning anything, tabulate **positions per regime month** across the
swept configs. If a directional strategy concentrates its trades in the regime
that opposes its direction (e.g. a long-only dip-buyer trading 5–6× more in
the bear month than the bull month), no parameter sweep can repair it. The
trigger fires exactly when the strategy cannot win. Stop sweeping and fix the
structure: add a trend filter, or move to futures so the other direction
exists. Note that shorting is a no-op on spot accounts — `DoShort` simply does
nothing there.

Report that conclusion plainly rather than shipping the least-bad parameter
set as a "win".

#### G) Inconsistent lab vs standalone results

1. Lab `tradeAmount` still at the 100.0 default while standalone used the real
   size → fix via `update_lab`, re-run.
2. `script_parameters` overrides silently reverted → compare
   `get_backtest_runtime` for both.
3. Different `position_mode` / `margin_mode` / interval → same check.
4. Different date ranges → verify unix timestamps match exactly.
5. Recompile changed compiled defaults between runs.

### Step 5 — Lab Result Interpretation (Cloud)

1. **Filter zero-trade configs first** (Step 4B), then rank.
2. **Don't crown the ROI leader blindly.** ROI 8% / MaxDD 3% / Sharpe 1.2
   beats ROI 10% / MaxDD 15% / Sharpe 0.3.
3. **Look for stability clusters.** Top results sharing a narrow parameter
   range = robust; scattered = noisy sweep, widen the search or add months.
4. **Cross-regime validation is mandatory.** One 31-day lab month proves
   nothing on its own. Clone the lab across at least bear + bull + sideways
   months and require the survivor to be acceptable in **all** of them
   (worst-month ROI is the ranking key).
5. **Validate the top 3–5 standalone** over the full license-cap horizon
   before declaring a winner.
6. **Judge the winner against buy-and-hold over the validation span**, not
   against zero. "Closest to break-even of 117 configs" is a FAIL if holding
   the asset returned +6%.
7. Combo drill-in is single-call: `lab_id` + the result entry's `BID` into the
   `get_backtest_*` tools. No re-run needed for analysis.
8. Full-factorial sweeps: algorithm type 0 with explicit value lists runs
   every combination — the `SB` field in the start response is the combo
   count. Check it before walking away (81 combos ≈ minutes; thousands ≈
   hours).

**Winner selection priority:** all gates passed → lowest fee drag → highest
Sharpe → most stable neighbourhood → highest ROI. In that order.

### Step 6 — Live Bot vs Backtest

Expected (not bugs): slightly worse live fills (slippage), fee-schedule
differences, order latency.

Red flags (investigate):
- Live win rate < 80% of backtest win rate
- Live trade frequency far below expected
- Live fees > 2× backtest assumption
- Recurring failed orders in `get_bot_runtime`

Workflow: `get_bot_runtime` → `get_bot_positions(status="closed", limit=20)`
→ `get_bot_open_orders` → `get_backtest_runtime` of the reference backtest to
confirm identical parameters. Remember: bots are read-only over the Cloud MCP;
any change the analysis recommends must be applied in the HTS web interface.

### Step 7 — Output Format

Every performance analysis must include:

```
## Performance Analysis: [Script / Lab / Bot name]
### Period: [range] | Market: [tag] | Account: [type]

### Headline Verdict
[One sentence: PASS / CONDITIONAL / FAIL and why]

### Key Metrics
| Metric | Value | Benchmark | Assessment |
|---|---|---|---|
| ROI | | > 0% | |
| Alpha vs hold | | > 0% | |
| Max Drawdown | | < 2× market DD | |
| Fee Drag Ratio | | < 0.40 | |
| Avg gross/trade vs round-trip cost | | > 1× | |
| Profit Factor | | > 1.3 | |
| Sharpe | | > 0.5 | |
| Win Rate | | strategy-dep. | |
| Trade Count | | > 0 | |

### Regime Gate Results
[Regime Validation Summary from Step 3]

### Primary Constraint
[One sentence]

### Root Cause Analysis
[2–4 sentences]

### Recommended Next Actions
1. [Most impactful, specific, actionable]
2. [Second, conditional on #1]
3. [Optional longer-horizon improvement]

### Parameters Used
[From get_backtest_runtime — what was actually tested]
```

---

## Quick Reference: Common Cloud Scenarios

**"Run a one-year backtest."**
→ Standalone only, and only on Pro (12 months) or TradeServer (36 months);
Standard caps at 6 months, Starter at 1. Never allowed as a lab — split lab
work into 31-day months on every tier.

**"Sweep parameters over six months."**
→ Impossible in one lab run. Pick 3 regime months (bear/bull/sideways),
`clone_lab` per month, sweep each, rank by worst-month ROI after filtering
zero-trade configs, then validate top configs standalone across the full span.

**"Backtest 2021 data?"**
→ Yes — the caps limit window *length*, not how far back it starts. Verify
data exists first with `check_market_data`.

**"Is this strategy ready for live?"**
→ All four regime gates must PASS. Any FAIL → no.

**"Why is my grid bot losing money?"**
→ Almost always fee drag. Compute the ratio and cost-vs-edge first. Fix with
fewer round trips / wider spacing / better fee rate — never "bigger orders".

**"Positive ROI but I'd have made more just holding."**
→ Compute alpha from actual candles. Bull-only bleed → trend filter.
Universal bleed → it's a hedge; say so.

**"Lab winner looks great, standalone disagrees."**
→ Check lab `tradeAmount` (default 100.0), then runtime parameter comparison,
then exact date ranges.

**"The results changed between two identical runs."**
→ `get_backtest_runtime` on both: resolved script parameters, futures params,
and whether a recompile changed compiled defaults in between.
