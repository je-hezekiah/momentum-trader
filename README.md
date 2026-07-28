# Momentum Trader

Autonomous momentum trading agent template for Solana, designed for the Hatcher platform (OpenClaw).

## Current Status: Phase A (Paper / Read-Only)

This version is intentionally restricted for safe testing:

- Runs in **paper mode** by default
- Does **not** execute real trades
- Does **not** use a funded wallet
- Only proposes trade ideas
- Short selling is currently **unsupported**

## Overview
Momentum Trader monitors selected Solana pairs, detects momentum breakouts confirmed by volume, and generates risk-managed trade proposals. It enforces position sizing, stop-loss rules, cooldowns, and drawdown limits.

## Framework
- **Runtime:** OpenClaw  
- **Category:** DeFi & Trading  
- **Network:** Solana  
- **License:** MIT  

## Repository Structure
| File | Description |
|------|-------------|
| `manifest.json` | Agent metadata |
| `SOUL.md` | System prompt and behaviour rules |
| `config.example.json` | Safe example configuration (no secrets) |
| `LICENSE` | MIT License |
| `README.md` | This file |

## Configuration
Copy `config.example.json` and adjust values as needed.

Key safety settings:
- `"mode": "paper"` → Forces paper / read-only behaviour
- `"allow_live_execution": false` → Prevents real trades
- `"allow_short": false` → Short selling is disabled

**Important:** Never commit private keys, API keys, or wallet secrets.

## How Configuration & State Work
- The agent loads settings from the configuration file at startup.
- Cooldowns, open paper positions, daily drawdown, and weekly drawdown should be tracked in memory or a lightweight store between runs.
- In Phase A these values are used only for simulation and logging.

## Required Integrations
- Solana RPC / price data
- Volume data source
- Hatcher’s managed tools (for any future live execution)

## Risks
- Even in paper mode, signals can be wrong.
- Momentum strategies carry market risk.
- Always review proposed trades carefully before any future live use.

## Testing (Phase A)
1. Run the agent in paper mode.
2. Verify it only produces trade proposals (no transactions).
3. Confirm stop-loss, cooldown, and drawdown logic appear in the logs.
4. Confirm short trades are never proposed.

## Author
Reborn (@Rbornn1)  
GitHub: [je-hezekiah](https://github.com/je-hezekiah)
