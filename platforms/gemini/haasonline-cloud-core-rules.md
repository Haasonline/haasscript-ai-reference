---
name: haasonline-cloud-core-rules
description: "Core foundation and shared operational rules for HaasOnline Cloud v2 and HaasScript workflows. Use when designing, debugging, compiling, backtesting, optimizing Labs, analyzing performance, monitoring live bots, or executing Cloud MCP tools."
---

# HaasOnline Cloud Core Rules

## Purpose
Foundational operational standards, tool discipline, HaasScript language rules, and authority hierarchy across all HaasOnline Cloud workflows.

## 1. Authority Hierarchy & Tool Discipline
- **Authority Order:** Live MCP Tool Output > `search_haasscript_docs` > Workflow Skills > Reference Docs > Examples.
- **Pre-Flight Health:** Execute `health_check` before starting any Cloud tool workflow.
- **Dynamic ID Resolution:** Resolve entity IDs dynamically (`list_scripts`, `list_accounts`, `list_markets`, `list_labs`, `list_bots`). Never fabricate or hardcode IDs.
- **Live Bot Policy:** Live bot control via MCP is strictly **READ-ONLY**. Never attempt to create, edit, start, stop, or delete live bots via MCP. All operational changes must occur in the HTS interface.
- **Tool Economy:** Do not call Cloud tools for purely conceptual, mathematical, or explanatory queries unless live verification is requested.

## 2. HaasScript Language Essentials
- **Lua-Based & Indexing:** Case-sensitive, 1-based indexing (`` is the newest closed bar).
- **Logging & Math:** Use `Log()` (never Lua `print()`). Use native math functions (`Abs`, `Round`, `Max`, `Min`, `Floor`, `Ceil`) instead of `math.*`.
- **Inputs & Parameters:** Declare all `Input()` fields at top level. Represent Lab-swept booleans as numeric `1/0`.
- **State & Offsets:** Persist state with `Save`/`Load`, resetting in `Finalize()`. Set `offset=1` when excluding the currently forming bar from historical lookbacks.
- **Risk Guards:** Enforce hard stop-loss logic before trailing or profit-locking logic on directional/leveraged strategies.

## 3. Backtesting & Labs Execution Standards
- **Pre-Flight Checks:** Verify compiled script validity, account compatibility (prefer simulated), and market data coverage with `check_market_data` before running.
- **Execution Tracking:** Generate a fresh UUID for `backtest_id`, store the returned `service_id`, and poll `get_backtest_status` until terminal.
- **Trade Log Inspection:** On standalone runs, `FinishedPositions` is empty by design; verify trade counts in `Reports.P.C` and pull trade details from `get_backtest_positions`.
- **Pagination Standard:** Use the returned `NP` cursor for all paginated endpoints (`get_lab_results`, `get_backtest_positions`, `get_backtest_logs`, `get_backtest_export_data`).
- **Labs Constraints:** Enforce 31-day Cloud window limits. In `update_lab`, define search dimensions using either explicit lists (`O`) or ranges (`range=[start,end,step]`), never both.