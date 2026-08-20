# Momentum Trader

Autonomous momentum trading agent template for Solana, designed for the Hatcher platform (OpenClaw).

## Current Status: Phase B Validated

Phase B1 (supervised $10 SOL/USDC entry-to-exit) has been successfully completed and accepted by the Hatcher team.

Live execution remains **disabled by default**.  
Autonomous schedule remains **off**.  
Kill switch is active.  
Wallet remains unfunded until Hatcher explicitly approves the next step.

## Phase B Validation Result (Test)

A controlled Phase B pilot was completed with Hatcher's supervision:

- One supervised SOL/USDC long position was opened and closed
- Entry: 9.978683 USDC → 0.118663549 SOL
- Exit: 0.118663549 SOL → 10.239204 USDC
- Realized PnL: +0.260521 USDC
- Both transactions were confirmed and reconciled
- SOL reserve was preserved
- No duplicate execution or reconciliation issues occurred
- Server-side $10 cap, single-position limit, idempotency, and state handling all functioned correctly

This was a supervised test under tight limits. It is **not** a performance claim or guarantee of future results.

### Phase B Hard Restrictions
- Pair: **SOL/USDC only**
- Direction: **Long only**
- Max notional per trade: **$10**
- Max open positions: **1**
- Realized daily loss limit: **$2**
- Max slippage: **75 bps**
- Live equity baseline: **$10** (excludes 0.04 SOL reserve)
- Single documented stop-loss: **8%**

### Key Safety Features
- Durable candle decision keys (idempotency)
- UUID persistence before every Jupiter intent
- Timeout-safe reconciliation (same UUID only — never a new execution)
- Dedicated `SKIP: DUPLICATE_MANUAL_CLOSE` key for manual closes
- Hard $10 policy limit (`LIVE_SKIP: POLICY_LIMIT`)
- Post-transaction reconciliation using signature + final balances
- Paper balance and live equity are fully separated
- Live drawdown calculated only against the $10 baseline

## Overview
Momentum Trader monitors selected Solana pairs, detects volume-confirmed momentum breakouts, and generates risk-managed trade proposals. It enforces position sizing, stop-loss rules, cooldowns, drawdown limits, and strict idempotency.

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
- `"max_position_usd": 10` → Hard Phase B limit
- `"live_equity_baseline_usd": 10` → Pilot live equity baseline
- `"realized_daily_loss_limit_usd": 2`
- `"stop_loss_pct": 8` → Single documented stop-loss rule

**Important:** Never commit private keys, API keys, or wallet secrets.

## How Configuration & State Work
- The agent loads settings from the configuration file at startup.
- Cooldowns, open positions, daily/weekly drawdown, decision keys, UUIDs, and pending intents are tracked in state.
- Evaluation locking prevents duplicate proposals/orders for the same closed 5-minute candle.
- Manual closes use a dedicated key and return `SKIP: DUPLICATE_MANUAL_CLOSE`.
- Timeouts after a mutation never create a new UUID or new execution attempt — only reconciliation with the original UUID is allowed.

### Routine Evaluation State Rule
On normal HOLD / SKIP evaluations the agent may only:
- Update `last_evaluation`
- Append an entry to the structured log

It must **not** modify decision keys, open positions, cooldowns, or any other trading state.

## Required Fixture Coverage
The following negative cases are supported:
- Over-limit size ($10.01 / $11) → `LIVE_SKIP: POLICY_LIMIT`
- Duplicate candle → `SKIP: DUPLICATE_CANDLE`
- Duplicate manual-close attempts → `SKIP: DUPLICATE_MANUAL_CLOSE`
- Transaction succeeded, but agent response timed out → reconciliation only (same UUID, not a new execution)

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

## Author
X: Reborn (@Rbornn1)  
GitHub: [je-hezekiah](https://github.com/je-hezekiah)
