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

Built on Lua, HaasScript includes **600+ custom commands** specifically designed for cryptocurrency trading operations.

## 🎯 Purpose of This Repository

This repository contains **AI-optimized documentation** for HaasScript, specifically formatted to help AI coding assistants (like Claude, ChatGPT, GitHub Copilot) provide better support when helping users write HaasScript code.

### Documentation Structure

```
├── AGENT.md                    # AI assistant guidance & best practices
├── HAASSCRIPT.md              # Technical reference overview
└── haasscript/
    ├── technical-analysis.md  # 157 indicator commands
    ├── trading.md             # 139 trading & position commands
    ├── helpers.md             # 121 utility commands
    ├── enumerations.md        # 197 constant values
    ├── data-and-prices.md     # 62 price & market data commands
    └── advanced.md            # 97 advanced feature commands
```

### For AI Assistants

Start with **AGENT.md** for:
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

- **Lua-based** with 600+ custom trading commands
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

*For AI assistants: This repository is optimized for your consumption. Start with AGENT.md for the best results when helping users with HaasScript.*
