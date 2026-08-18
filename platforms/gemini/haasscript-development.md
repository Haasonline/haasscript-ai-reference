---
name: haasscript-development
description: "Instructions for creating, modifying, compiling, debugging, and regression-testing HaasScript strategies on HaasOnline Cloud v2. Use when authoring scripts, patching compiler errors, diagnosing zero-trade backtests, or managing script backups."
---

# HaasScript Development

## Purpose
Guide the creation, modification, compilation, debugging, and regression testing of HaasScript on HaasOnline Cloud v2.

## 1. Task Routing & Safety Protocol
- **New Script Creation:** Implementation plan -> write code -> `compile_script` -> `add_script` -> verify with `get_script`.
- **Code Modification Safety:**
  1. Fetch current script via `get_script`.
  2. Create a named semantic backup via `add_script` (e.g., `Strategy-v2.1-backup`).
  3. Apply code changes.
  4. Validate syntax with `compile_script`.
  5. Save updates with `edit_script_source`.
  6. Confirm state with `get_script` and run regression backtests.
- **Verification Rule:** Never state code is fixed or compiled without tool/compiler confirmation.

## 2. Script Architecture Checklist
- Top-level `Input()` declarations.
- Price data and indicator calculations (`offset=1` for closed-bar indicators).
- Signal, entry, and exit logic.
- Position and open-order guards.
- Trade execution (prefer managed execution).
- State persistence (`Save`/`Load` and reset inside `Finalize()`).
- Plotting and `Log()` diagnostics.

## 3. Systematic Zero-Trade Diagnostic Hierarchy
When a backtest completes with zero trades, investigate in this exact sequence:
1. **Logs:** Inspect `get_backtest_logs` for execution errors, sync failures, or rejected order events.
2. **Runtime:** Check `get_backtest_runtime` for actual compiled inputs, margin modes, and failed orders.
3. **Market Data:** Verify price data coverage and gap flags via `check_market_data`.
4. **Account & Sizing:** Check min/max lot sizes, account leverage, and order template limits.
5. **Logic Guards:** Inspect entry conditions, indicator math, and position guards for unreachable states.