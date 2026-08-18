# HaasOnline Cloud Core Rules

## Purpose

Shared rules for all HaasScript Cloud workflows. This file is the common
layer used by development, performance-analysis, and instrumentation
skills.

## Authority and freshness

1.  Current HaasBot Cloud tool schemas and direct tool results are
    authoritative for Cloud behavior.
2.  `search_haasscript_docs` is authoritative for HaasScript command
    names, signatures, parameters, and return types when uncertain.
3.  Uploaded/reference documentation is the canonical language/reference
    layer unless superseded by current tool evidence.
4.  Examples are patterns, not authority.
5.  Label changing observations as verified/observed rather than
    treating them as permanent platform guarantees.

## Tool discipline

-   For tasks requiring Cloud tools, run `health_check` first.
-   Do not call Cloud tools for purely explanatory questions unless live
    verification is needed.
-   Resolve scripts, accounts, markets, Labs, and bots with the relevant
    list tools. Never invent or ask for IDs the connector can discover.
-   Use the minimum calls required and reuse results already obtained.
-   Never claim a tool action occurred unless the tool actually
    performed it.

## Live bots

MCP live-bot access is read-only. Observe runtime, positions, orders,
logs, profits, and reports. Do not claim to create, edit, start, stop,
or delete a live bot through MCP. Direct changes to the HTS interface
when necessary.

## Backtest evidence

A terminal execution state means execution ended, not that trading
succeeded. For standalone backtests: - `get_backtest_runtime` is
authoritative for runtime/report state. - `Reports.P.C` is the
closed-position count. - `FinishedPositions` is intentionally empty on
the standalone runtime path; use `get_backtest_positions` for trades. -
Read `get_backtest_logs` before reporting success. - Use
`get_backtest_positions` to verify actual trades. For Lab combos, pass
both `lab_id` and the result `BID` to the backtest drill-in tools.

## Backtest pre-flight

Before a backtest: 1. Resolve a compatible account and market. 2. Verify
the script is compiled and intended inputs/settings are applied. 3.
Check market-data coverage with `check_market_data`; sync only when
required. 4. Keep the requested window within the server/license limit.
5. Build complete, validated settings. `execute_backtest` currently
treats `orderTemplate` as optional and defaults invalid/absent values to
`500`; prefer an explicit known-good value when constructing settings
manually. 6. Generate a fresh backtest UUID and store the exact
`service_id` returned by `execute_backtest`. 7. Poll the returned
service until terminal.

## Pagination

Follow each endpoint's documented `NP` cursor. Do not invent cursors. -
`get_lab_results`: use `NP`; a fresh snapshot can move page boundaries
while execution is running, so fetch a stable page after completion. -
`get_backtest_positions` and logs: paginate with returned `NP`. -
`get_backtest_export_data`: paginate rows with returned `NP`; the file
is not limited to one response. - Keep pages small enough for MCP
transport.

## HaasScript essentials

-   Lua-based, case-sensitive, arrays are 1-based; `[1]` is the newest
    value.
-   Use `Log()`, not Lua `print()`.
-   Do not use `math.*` for HaasScript numeric values; use HaasScript
    functions such as `Abs`, `Round`, `Max`, `Min`, `Floor`, `Ceil`.
-   Put `Input()` calls at top level.
-   Lab-swept booleans use numeric `1/0`.
-   Use `Save/Load` for persistent state and reset state in `Finalize()`
    where appropriate.
-   Use position/order guards before managed trade actions.
-   Directional/leverage strategies should have explicit loss control
    unless the user deliberately requests otherwise.
-   Use `offset=1` when a historical lookback must exclude the forming
    bar.
-   Verify uncertain commands with `search_haasscript_docs`.

## Change safety

For a material change to an existing script: 1. `get_script` 2. create a
named backup with `add_script` 3. compile/update 4. verify with
`get_script` 5. regression-test when behavior changed. Do not claim a
script is fixed or compiled without evidence.

## Performance principles

Every strategy-performance analysis should state: - Verdict: PASS /
CONDITIONAL / FAIL - Primary constraint - Evidence-backed root cause -
Concrete next action Compare strategy ROI with buy-and-hold over the
same period. Consider alpha, drawdown, fees, profit factor, Sharpe,
trade count, sample size, and robustness. Do not declare production
readiness from one backtest or one Lab winner.

## Strategy classification

Before judging performance, identify the strategy class where possible:
trend, momentum, mean reversion, grid, DCA, market making, scalping,
arbitrage, long-only spot, directional futures, or hybrid. Use
class-appropriate benchmarks and thresholds. Numeric gates are
heuristics unless directly supported by the strategy requirements.

## Tool-use economy

Use the minimum evidence needed to answer the question accurately.
Classify the task first: - Explain/review → usually no Cloud call. -
Create/modify → development workflow. - Compile/debug → compile +
targeted diagnostics. - Backtest/Lab → full pre-flight. - Analyze
results → collect only the evidence required. - Live monitoring →
read-only observation. - Instrumentation → export workflow.
