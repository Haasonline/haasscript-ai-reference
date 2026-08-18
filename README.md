# HaasScript AI Reference Documentation

Comprehensive reference documentation for **HaasScript**, a Lua-based scripting language designed for creating automated cryptocurrency trading strategies on [Haasonline's](https://www.haasonline.com/) trading platform.

## 📚 What is HaasScript?

HaasScript is a powerful scripting language that enables traders to:
- Create custom automated trading strategies
- Access real-time and historical price data
- Calculate technical indicators (RSI, MACD, Bollinger Bands, etc.)
- Manage positions and orders programmatically
- Implement risk management (stop loss, take profit, trailing stops)
- Create custom visualizations and charts

Built on Lua, HaasScript includes **750+ built-in commands** specifically designed for cryptocurrency trading operations.

## 🎯 Purpose of This Repository

This repository contains **AI-optimized documentation** for HaasScript, specifically formatted to help AI coding assistants (like Claude, ChatGPT, GitHub Copilot) provide better support when helping users write HaasScript code.

### Documentation Structure

```
├── project-instructions/       # Drop-in system prompts for an MCP-connected assistant
│   ├── cloud.md               #   HaasOnline Cloud (app.haasbot.com)
│   └── self-hosted.md         #   Self-hosted / managed TradeServer
├── skills/                     # Claude Skills — deeper, task-specific workflows
│   ├── haasscript-development/
│   ├── haasscript-performance-analysis-cloud/
│   └── haasscript-export-instrumentation-cloud/
├── AGENTS.md                   # AI assistant guidance & best practices (non-MCP sessions)
├── HAASSCRIPT.md               # Technical reference overview
└── haasscript/
    ├── technical-analysis.md  # 157 indicator commands
    ├── trading.md             # 136 trading & position commands
    ├── helpers.md             # 118 utility commands
    ├── enumerations.md        # 198 constant values
    ├── data-and-prices.md     # 62 price & market data commands
    └── advanced.md            # 97 advanced feature commands
```

## 🔌 Using this with an MCP-connected assistant

If you have connected Claude, Cursor, VS Code or another MCP client to your
HaasOnline server ([setup guide](https://help.haasonline.com/cloud-edition/mcp-server/)),
the files below turn a generic assistant into one that knows the platform's
workflows, limits and failure modes.

### Project instructions

Pick **one** and paste it into your assistant's system prompt / custom
instructions (in Claude, that's a Project's "Instructions" box):

| File | Use when |
|---|---|
| [`project-instructions/cloud.md`](project-instructions/cloud.md) | You're on **HaasOnline Cloud** — Starter, Standard or Professional |
| [`project-instructions/self-hosted.md`](project-instructions/self-hosted.md) | You run **TradeServer** on your own hardware or a managed host |

The two editions differ in more than branding: the Cloud MCP exposes a
different tool surface (no `clone_script`, no `compare_backtests`, no
filesystem access to exported files) and enforces different execution caps.
Using the wrong one produces tool calls that fail.

### Skills

Skills are loaded *on demand* — the assistant reads one only when the task calls
for it, so they can go far deeper than a system prompt's budget allows. The two
`-cloud` skills target **Cloud**; `haasscript-development` covers both Cloud and
self-hosted TradeServer, flagging where the two differ.

| Skill | What it covers |
|---|---|
| [`haasscript-development`](skills/haasscript-development/SKILL.md) | Writing, modifying and debugging scripts: the silent-failure list, the pre-compilation lint checklist, the compile→debug cycle, diagnosing zero-trade backtests, and verified patterns |
| [`haasscript-performance-analysis-cloud`](skills/haasscript-performance-analysis-cloud/SKILL.md) | Interpreting backtest, lab and live-bot results: regime windows, fee drag, alpha vs ROI, and what to change next |
| [`haasscript-export-instrumentation-cloud`](skills/haasscript-export-instrumentation-cloud/SKILL.md) | Capturing per-decision feature vectors from a backtest and joining them into a tuning dataset |

**Installing them in Claude Code or Claude Desktop.** Skills live in a directory
named after the skill, each containing a `SKILL.md` — which is exactly how this
repo stores them, so you can link them straight in:

```bash
git clone https://github.com/Haasonline/haasscript-ai-reference.git
ln -s "$PWD/haasscript-ai-reference/skills/"* ~/.claude/skills/
```

Or copy a single one:

```bash
cp -r haasscript-ai-reference/skills/haasscript-development ~/.claude/skills/
```

For project-scoped use instead, put them under `.claude/skills/` in your own
repo. Other MCP clients that support skills use the same `SKILL.md` convention;
for those that don't, the files are still readable as plain Markdown guides.

> **Note on limits.** These documents quote plan limits (backtest windows, bot
> counts, Labs availability) verified against a production Cloud server in July
> 2026. Limits change — treat [haasonline.com/pricing](https://haasonline.com/pricing)
> as authoritative, and note that the skills recommend *probing* your server's
> real cap rather than assuming, since a rejected backtest errors instantly and
> costs nothing.

### For AI Assistants

If connected to a live HaasOnline server via MCP, start with the matching file
in **project-instructions/** for:
- MCP tool workflows and verified parameter signatures
- Backtesting, lab execution, and live bot monitoring patterns
- All core HaasScript language guidance

Otherwise, start with **AGENTS.md** for:
- Best practices and common patterns
- Language-specific gotchas (1-based indexing, `Log()` vs `print()`, etc.)
- Trading approach guidance (managed vs unmanaged)
- Example code templates

### For Developers

Reference **HAASSCRIPT.md** for:
- Type system overview
- Execution model
- Command index by domain

Then dive into domain-specific documentation in the `haasscript/` directory as needed.

## 🔑 Key Language Features

- **Lua-based** with 750+ built-in commands
- **1-based array indexing** (not 0-based like most languages)
- **Per-minute execution** by default (configurable to 5-10 seconds)
- **Managed trading mode** for simplified position management
- **Unmanaged trading mode** for full control over orders
- **Custom types**: `HaasNumberCollection`, `HaasSignal`, `HaasEnum`

## 📖 Quick Example

```lua
-- Simple RSI strategy
local rsiLength = Input("RSI Length", 14)
local buyLevel = Input("Buy Level", 30)
local sellLevel = Input("Sell Level", 70)

local rsi = RSI(ClosePrices(), rsiLength)

if rsi < buyLevel then
    DoLong("RSI Oversold")
elseif rsi > sellLevel then
    DoShort("RSI Overbought")
end
```

## 🤖 Pre-Configured AI Assistants

Get instant HaasScript coding help with these pre-configured AI assistants that have been loaded with the complete documentation:

### ChatGPT
**[HaasScript Guru](https://chatgpt.com/g/g-krwaYRGz6-haasscript-guru)** - Custom GPT for HaasScript
- Pre-loaded with all command references and best practices
- Interactive coding assistance and debugging
- Strategy development and optimization help

### Google Gemini
**[HaasScript Guru](https://gemini.google.com/gem/1v1DKME3LdPtgw_rYg42gSJXsY1fRmdf6?usp=sharing)** - Gemini Gem for HaasScript
- Complete documentation access
- Instant access - just start asking questions
- Quick coding help and strategy development

*More AI assistant packages for other platforms coming soon!*

## 🔗 Resources

- **Official Documentation**: [help.haasonline.com/haasscript](https://help.haasonline.com/haasscript/general/)
- **Haasonline Platform**: [haasonline.com](https://www.haasonline.com/)
- **Community Scripts**: [HaasScripts.com](https://haasscripts.com)
- **Community Support**: [Haasonline Discord](https://haas.bot/discord)

## 📄 License

This documentation is provided for reference purposes. For licensing information about HaasScript and Haasonline TradeServer, please visit [haasonline.com](https://www.haasonline.com/).

---

*For AI assistants: This repository is optimized for your consumption. If connected to a live HTS via MCP, start with `project-instructions/cloud.md` or `project-instructions/self-hosted.md` as appropriate. Otherwise, start with AGENTS.md.*
