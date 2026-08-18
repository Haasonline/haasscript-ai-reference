---
name: haasscript-performance-analysis
description: "Analyzes HaasOnline Cloud v2 backtests, Labs sweeps, and live bot performance. Use when evaluating backtest results, calculating alpha vs buy-and-hold, diagnosing fee drag, reviewing Labs parameter sweeps, or determining strategy production readiness."
---

# HaasScript Performance Analysis

## Purpose
Quantitative evaluation of backtests, Labs sweeps, and bot runtimes to diagnose performance bottlenecks and deliver a structured verdict: PASS, CONDITIONAL, or FAIL.

## 1. Quantitative Diagnostics
- **Alpha vs. Buy-and-Hold:**
  - Hold ROI % = ((Ending Close / Starting Close) - 1) * 100
  - Alpha % = Strategy ROI % - Hold ROI %
  - *(For futures/leveraged/short strategies, mark spot buy-and-hold as reference only).*
- **Fee Drag:**
  - Fee Drag % = (Total Fees / Gross Profit) * 100 (when Gross Profit > 0)
  - High fee drag + high turnover: widen edge target or reduce trade frequency. Never scale position size to fix fee drag.
- **Sample Robustness:** Samples under 30 trades represent unproven hypotheses. Never declare production readiness from an isolated run or single Lab top-rank.

## 2. Labs & Sweep Evaluation
- Discard failed, partial, or zero-trade configurations before ranking.
- Prioritize parameter neighborhood stability and multi-regime robustness over isolated ROI spikes.
- **Subagent Delegation:** When analyzing extensive parameter sweeps or multi-month execution logs across multiple assets, spawn parallel subagents to evaluate independent subsets and synthesize findings.

## 3. Required Output Format
```text
## Performance Analysis
Strategy: [Name / Version]
Class: [Trend / Mean Reversion / Grid / DCA / Scalping]
Period: [Start Date] to [End Date]
Market: [Pair / Exchange / Account Type]

### Verdict
[PASS / CONDITIONAL / FAIL]

### Key Metrics
ROI: [Value]% | Buy-and-Hold: [Value]% | Alpha: [Value]%
Max Drawdown: [Value]% | Fee Drag: [Value]% | Profit Factor: [Value]
Sharpe Ratio: [Value] | Trade Count: [Count] | Win Rate: [Value]%

### Primary Constraint & Root Cause
[Primary bottleneck (e.g., Fee Drag, Tail Risk, Regime Decay) and evidence from logs]

### Recommended Next Actions
1. [Actionable step 1]
2. [Actionable step 2]
3. [Actionable step 3]