---
name: haasscript-export-instrumentation
description: "Instructions for capturing decision-edge telemetry in HaasScript, retrieving export rows via Cloud MCP, joining features to realized trade outcomes, and generating testable optimization hypotheses. Use when instrumenting HaasScript strategies, configuring InitExportData or WriteExportData, capturing decision-time feature vectors, retrieving paginated backtest exports via get_backtest_export_data, joining signals to closed trade positions from get_backtest_positions, or running feature distribution analysis."
---

# HaasScript Export Instrumentation

## Purpose
Capture state values at true entry/exit decision points, retrieve export datasets via Cloud MCP, join features to realized trade positions, and construct testable parameter optimization hypotheses.

---

## 1. Core Principles & Capture Patterns

- **Decision-Edge Capture:** Log state immediately prior to committing to an order action. Do not bind telemetry capture solely to managed position state changes when analyzing signal quality.
- **Full Feature Vectors:** Record all relevant inputs, indicator values, threshold boundaries, state flags, decision-time price, and trade direction.
- **Capture Patterns:**
  - **Decision Events (Standard):** Log `ENTRY` and `EXIT` rows strictly on `false -> true` condition transitions. This is the required pattern for parameter optimization workflows.
  - **Per-Tick / Per-Bar Series:** Use only when continuous inter-decision indicator tracking is required. Enforce floating-point rounding and cadence gating.
  - **Scalar Metrics:** Use `CustomReport()` for end-of-run aggregate numbers.
- **Additive Integrity:** Instrumentation must never modify underlying strategy logic. Reset persistent `Save`/`Load` trigger flags in `Finalize()`.

---

## 2. HaasScript Export Syntax & Code Templates

### Schema Initialization (`InitExportData`)
Define explicit, descriptive column headers. Ensure column count strictly matches `WriteExportData`.

```lua
-- Example InitExportData pattern in top-level init scope
InitExportData("DecisionTelemetry", {
    "Timestamp", "EventType", "Price", "RSI", "FastMA", "SlowMA", "SignalDiff", "Direction"
})