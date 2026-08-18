# HaasScript Development --- HaasOnline Cloud v2

## Purpose

Create, modify, compile, debug, and regression-test HaasScript on
HaasOnline Cloud. Use this skill for new strategies, material code
changes, compile errors, zero-trade debugging, and strategy execution
problems.

Shared Cloud rules are in `HaasOnline Cloud Core Rules`. Do not
duplicate them unless a rule is essential to the immediate workflow.

## 1. Task classification

-   Explanation/review only: answer from the supplied code/reference;
    use Cloud only when live verification is necessary.
-   New script: implementation plan → code → compile → save → verify.
-   Material modification: backup → plan → compile → save → verify →
    regression test.
-   Compile/debug: reproduce/compile → inspect errors → fix → recompile.
-   Zero trades: logs/runtime/data/configuration first, then script
    logic.
-   Risk/order-flow change: require regression testing and appropriate
    regime validation.

Do not block an already-authorized, sufficiently clear change merely to
obtain redundant approval. Ask only when the requested behavior is
materially ambiguous or unsafe.

## 2. Before coding

Resolve what is actually known from the conversation first. Determine: -
strategy and entry logic - exit logic and loss control - spot vs
margin/futures - managed vs unmanaged execution - interval and sizing -
inputs intended for Lab sweeps - whether the change is behavioral or
cosmetic

For uncertain HaasScript commands, call `search_haasscript_docs` before
writing them. Treat the result as authoritative.

## 3. Script structure

Prefer: 1. top-level Inputs 2. price/indicator calculations 3.
signal/entry/exit conditions 4. position and order guards 5.
managed/unmanaged execution 6. persistent state 7. reports/Finalize

Keep meaningful tunables as `Input()` parameters. Use `1/0` for
Lab-swept booleans.

## 4. Compile-first workflow

For a draft: 1. `compile_script(source_code)` to validate without
saving. 2. Inspect `isValid`, compiler errors, and discovered input
fields. 3. For new scripts use `add_script`. 4. For existing scripts use
`edit_script_source`. 5. `get_script` to verify the saved source and
compile state.

When a Lab is intended, verify that swept Inputs are exposed with the
expected numeric type/defaults before building the Lab.

## 5. Material-change safety

Before changing an existing script: 1. `get_script` 2. `add_script`
named backup 3. make the change 4. compile 5. save/update 6. verify with
`get_script` 7. regression-test the behavioral change.

Use semantic version names such as `Strategy-v2.1.0-stop-fix`.

## 6. Lint checklist

Critical: - arrays start at 1 - no `math.*` - top-level Inputs - Lab
booleans are `1/0` - every persistent key is loaded with a default and
saved on intended paths - historical lookbacks use correct offsets -
position/order guards prevent duplicate managed actions - uncertain
commands were verified

Important: - set interval explicitly in backtest settings - set leverage
explicitly where relevant - ensure trade amount satisfies the selected
market - do not assume `scriptParameters` overrides were applied; verify
runtime inputs - keep Log output concise and diagnostic

## 7. Backtest settings

Build settings from a known-good configuration when possible. Current
Cloud `execute_backtest` accepts `orderTemplate` as optional and
defaults invalid/absent values to `500`; use an explicit validated value
when constructing a settings block.

Remember `tradeAmount` is interpreted according to the market/account
configuration, not as a universal quote-currency dollar amount.

## 8. Backtest workflow

Before executing: - compatible account - validated market - compiled
script - intended inputs - market-data coverage - interval - trade
amount - leverage/modes when relevant - valid time window

Then: 1. generate a fresh backtest UUID 2. `execute_backtest` 3. store
exact returned `service_id` 4. poll `get_backtest_status` 5. read
`get_backtest_logs` 6. read `get_backtest_runtime` 7. use `Reports.P.C`
for standalone closed-count 8. use `get_backtest_positions` for actual
trade details.

Never interpret a completed status alone as trading success.

## 9. Zero-trade debugging order

1.  Logs: execution errors, syncing/data failures, rejected order
    actions.
2.  Runtime: actual Input values, position/margin mode, failed/open
    orders.
3.  Market data: coverage and zero-volume periods.
4.  Account/market compatibility and minimum size.
5.  Interval/leverage.
6.  Entry conditions and position guards.

For futures/short strategies, verify the account supports the required
direction. Spot cannot execute short exposure.

## 10. Regression testing

For a behavioral change, reproduce the same baseline: - script version -
account - market - interval - trade amount - leverage/modes - exact date
range.

Compare actual metrics and trade details. Do not invent universal
tolerances; judge variance in context of the strategy and requested
change.

## 11. Labs

Use Labs for parameter sweeps, not as a substitute for structural
debugging. - Keep Lab windows within the current server limit. - Set
intended trade amount explicitly. - Use `O` for explicit values or
`range` for generated ranges, not both. - Filter failed/partial and
zero-trade results before ranking. - Prefer robust neighborhoods and
worst-regime behavior over isolated ROI peaks. - Drill a result using
both `lab_id` and its `BID`.

## 12. Risk

Directional/leverage strategies need explicit loss control unless
deliberately omitted. Prefer a hard stop before trailing/locking logic.
For grids, DCA, market making, and high-frequency strategies, explicitly
inspect exposure, fee drag, and edge per trade.

## 13. Output

For completed code work provide: - script name/version - concise logic
summary - Inputs table - code - validation status - tests performed or
recommended.

State exactly what was verified. Never say "fixed", "compiled", or
"backtested successfully" without the corresponding evidence.
