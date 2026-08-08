# Deterministic Cooldown Fixture Test Instructions

This document describes the required Phase A deterministic fixture test.

## Goal
Validate that:
1. A PROPOSAL correctly starts a cooldown
2. A subsequent evaluation within the cooldown window returns SKIP
3. Cooldown validity is calculated from stored timestamps (not external instruction)

## Procedure

### Step 1 – Generate PROPOSAL (t0)
Instruct the agent to treat the provided 5-minute OHLCV data as a valid clean momentum breakout with volume confirmation on SOL/USDC and produce a paper PROPOSAL.

The agent must:
- Calculate entry price and stop-loss
- Update `last_evaluation`
- Update `last_proposal_timestamps`
- Start cooldown (cooldown_seconds = 120)
- Not call any trading tool

### Step 2 – Cooldown Check (before t0 + 120s)
Immediately (or within the 120-second window) run a normal evaluation.

Expected result: SKIP because cooldown is still active.
- Only `last_evaluation` may be updated
- `last_proposal_timestamps` and cooldown must remain unchanged

### Step 3 (Optional) – After Expiry
Run another evaluation after the cooldown has expired.
Expected result: Proposals become eligible again (subject to signal quality).

## Important Rules
- Use actual timestamped 5-minute OHLCV candles + volume in the fixture
- Keep fixture state/logs separate from the live paper-observation state
- Or reset the paper state to a clean baseline after the test
- No valid 5-minute data → SKIP (never fall back to 24h aggregates)
