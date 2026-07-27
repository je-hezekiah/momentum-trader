# Momentum Trader

Autonomous momentum trading agent template for Solana, designed for the Hatcher platform (OpenClaw).

## Overview
Momentum Trader monitors selected Solana pairs, detects momentum breakouts confirmed by volume, and executes risk-managed long and short trades. It enforces strict position sizing, stop-loss rules, and drawdown limits.

## Framework
- **Runtime:** OpenClaw  
- **Category:** DeFi & Trading  
- **Network:** Solana  

## Repository Structure
| File | Description |
|------|-------------|
| `manifest.json` | Agent metadata |
| `SOUL.md` | System prompt and behaviour rules |
| `config.example.json` | Safe example configuration (no secrets) |
| `README.md` | Setup and usage instructions |

## Setup
1. Copy `config.example.json` and adjust parameters according to your risk tolerance.
2. Never commit private keys, API keys, or wallet secrets.
3. Supply sensitive credentials securely through the Hatcher platform or environment variables.
4. Deploy the agent via Hatcher (OpenClaw).

## Required Integrations
- Solana RPC endpoint
- Price and volume data
- Trade execution capability (provided through Hatcher / OpenClaw)

## Expected Environment Variables
- `SOLANA_RPC_URL` (or platform-managed equivalent)
- Execution credentials (managed securely, never stored in the repository)

## Risks & Permissions
- This agent can open and close trades using the configured capital.
- It will enforce the risk parameters defined in the configuration.
- Always begin with small position sizes.
- Momentum strategies carry market risk. Past behaviour does not guarantee future results.
- Standard Solana execution and smart contract risks apply.

## Testing Recommendations
1. Start with minimal position sizes.
2. Verify stop-loss and trailing-stop behaviour.
3. Confirm cooldown and drawdown limits function correctly.
4. Monitor initial live trades closely before scaling.

## Author
Name: Joe-Thomas Prince
X: (https://x.com/rbornn1)
Email: rebornonx@gmail.com
Github: https://github.com/je-hezekiah/momentum-trader
