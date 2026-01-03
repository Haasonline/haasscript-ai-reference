# HaasScript Documentation

Complete reference documentation for HaasScript commands and API.

## Quick Start

- **Start Here**: See [`AGENT.md`](../AGENT.md) for AI agent guidance, best practices, and common patterns
- **Technical Reference**: See [`HAASSCRIPT.md`](../HAASSCRIPT.md) for type system, execution model, and command index
- **Detailed Commands**: Browse domain files below for complete command documentation

## Command Domains

Documentation is organized into logical domains for easy navigation:

| Domain | Commands | Documentation |
|--------|----------|---------------|
| Advanced Features | 97 | [View →](advanced.md) |
| Data & Prices | 62 | [View →](data-and-prices.md) |
| Enumerations | 197 | [View →](enumerations.md) |
| Helpers & Utilities | 121 | [View →](helpers.md) |
| Technical Analysis | 157 | [View →](technical-analysis.md) |
| Trading & Positions | 139 | [View →](trading.md) |

**Total:** 773 commands across 6 domains

## File Count

**8 files total** (optimized for AI tool limits like Gemini's 8-file maximum):

1. `AGENT.md` - AI agent guidance and best practices (entry point)
2. `HAASSCRIPT.md` - Technical reference with overview and quick reference
3. `advanced.md` - Advanced Features
4. `data-and-prices.md` - Data & Prices
5. `enumerations.md` - Enumerations
6. `helpers.md` - Helpers & Utilities
7. `technical-analysis.md` - Technical Analysis
8. `trading.md` - Trading & Positions

## Regenerating Documentation

This documentation is auto-generated from the HaasScript source code. To regenerate:

```bash
dotnet run --project Source/Tools/HaasScriptDocGenerator
```

