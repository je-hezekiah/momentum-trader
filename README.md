# Momentum Trader

Autonomous momentum trading agent template for Solana, designed for the Hatcher platform (OpenClaw).

## Current Status: Phase B Preparation (Limited Live Spot Validation)

Phase A (24-hour paper observation) has been successfully completed and approved by the Hatcher team.

Live execution is **disabled by default**.  
A kill switch is active.  
Do **not** fund the wallet or enable autonomous live trading until Hatcher reviews the updated commit and coordinates the first supervised transaction.

### Phase B Hard Restrictions (first supervised validation)
- Pair: **SOL/USDC only**
- Direction: **Long only** (spot)
- Max notional per trade: **$10**
- Max open positions: **1**
- Realized daily loss limit: **$2**
- Max slippage: **75 bps**
- Single documented stop-loss: **8%** (from config)

### Key Safety Features Added for Phase B
- Evaluation locking / idempotency (no duplicate orders for the same closed candle)
- Retry handling that fails safely to SKIP without changing trading state
- Live execution mode disabled by default + kill switch
- Pre-execution validation of balance, quote, liquidity, price freshness, and expected output
- Structured execution logs (signal candle, decision, quote, transaction signature, result, state changes)

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
- `"allow_live_execution": false` → Prevents real trades (default)
- `"kill_switch": true` → Blocks all live actions when true
- `"allow_short": false` → Short selling is disabled
- `"stop_loss_pct": 8` → Single documented stop-loss rule
- `"max_position_usd": 10` → Phase B hard limit
- `"realized_daily_loss_limit_usd": 2` → Phase B hard limit

**Important:** Never commit private keys, API keys, or wallet secrets.

## How Configuration & State Work
- The agent loads settings from the configuration file at startup.
- Cooldowns, open positions, daily/weekly drawdown, and evaluation locks are tracked in state between runs.
- Evaluation locking prevents duplicate proposals/orders for the same closed 5-minute candle.
- Retries and overlapping runs fail safely to SKIP without modifying trading state.

## Required Integrations
- Solana RPC / price data
- Volume data source (Binance 5m klines can be used for signals)
- Hatcher’s managed tools (required for any future live execution)
- Every live transaction must be re-validated against the actual Solana execution quote

## Risks
- Even in paper mode, signals can be wrong.
- Momentum strategies carry market risk.
- Always review proposed trades carefully before any future live use.
- Live mode remains disabled until explicitly enabled by Hatcher.

## Testing
### Phase A (Completed)
- Paper-only operation
- Correct SKIP behaviour
- Persistent state
- No trading tools or signatures

### Phase B Preparation
- Live mode remains disabled
- Hard restrictions listed above are enforced
- Idempotency and safe retry behaviour are required

## Author
X: Reborn (@Rbornn1)  
GitHub: [je-hezekiah](https://github.com/je-hezekiah)
