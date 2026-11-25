---
name: liquidity-sweep-model
description: Grade a stock or ETF setup with my liquidity sweep trading model (sweep, FVG, market structure shift) using live Robinhood data, with sizing and guardrails.
---

# Liquidity Sweep Trading Model (v1)

Use this skill when I ask to check, grade, or find a setup with "my model", "the sweep model", "ICT setup", "FVG entry", or ask whether a ticker has a valid long/short entry under my strategy.

Analysis, not advice. I make every decision. This model is in its **paper-trading phase**: never place an order from this skill. Only review an order if I explicitly ask, and then follow the project ORDERS rules (review, show, STOP, place only on the exact words PLACE THE ORDERS, agentic account only).

Bullish rules are shown. For bearish, flip every rule (premium instead of discount, sweep of highs, bearish FVG, close below a swing low, stop above the sweep high).

## Procedure

### 0. Connector and context
- Load Robinhood tools with ToolSearch. Call get_accounts. If the connector isn't signed in, tell me and stop.
- Accounts: agentic (Robinhood nickname "MCP") is the only tradable account; main account is read-only context.
- Read the project doc `claude/trading-model.md` if it exists; if it differs from this skill, the doc wins and tell me about the difference.

### 1. Pull live data (never use memory)
- Quote: get_equity_quotes (say if intraday or after-hours).
- Daily bars ~1 year: get_equity_historicals interval day. 50-day SMA: get_equity_technical_indicators (sma, period 50).
- 1-hour bars for the last ~20 sessions; 5-minute bars for today and the prior session. If an interval isn't available, say so and grade only the steps you can.
- Earnings: get_earnings_results. Market context: SPY quote; VIX if available (get_index_quotes), otherwise say unavailable.

### 2. Step 1 — Market profile (daily)
- Trending up: price above a rising 50-day SMA AND the last two confirmed daily swing highs and lows are higher. Longs only.
- Trending down: the reverse. Shorts only (puts or no trade in accounts that can't short).
- Range: anything else. Only trade sweeps of the range high/low back into the range.

### 3. Step 2 — Dealing range (1-hour)
- Swing high = candle with 2 lower highs on each side; swing low = candle with 2 higher lows on each side.
- Dealing range = most recent confirmed swing high and swing low.
- Discount = below 50%. Best zone = 62–79% retracement. No trades in the 45–55% band.
- Longs only in discount; shorts only in premium. Report where price sits as a % of the range.

### 4. Step 3 — Draw on liquidity
Mark: previous day high/low, previous week high/low, previous month high/low, unfilled 1-hour FVGs. The draw must be in the trade's direction with no major level blocking it.

### 5. Step 4 — Entry (5-minute), all in order
1. Sweep: a candle wicks beyond a marked level (prior day low, premarket low, or 1-hour swing low) and closes back inside within 2 candles.
2. FVG after the sweep: 3-candle gap (candle 3 low > candle 1 high for bullish), width at least 0.1% of price.
3. Market structure shift: a 5-minute candle CLOSES beyond the swing high formed between the sweep and the FVG. A wick does not count.
4. Entry: limit at the 50% level of the FVG.
5. Stop: 0.05% of price beyond the sweep extreme.
6. Cancel if not filled within 12 five-minute candles, or if price hits the target first.

### 6. Step 5 — Exits
- Target = the draw on liquidity. Skip the trade if target < 2R.
- Take half at 1.5R and move the stop to breakeven.
- Intraday: close 15 minutes before the close. Swing: exit if the target isn't hit within 5 sessions.
- Never widen a stop. Never add to a loser.

## Guardrails (check every one)

**A. Risk and size**
- Risk 1% of the account's current value per trade. Size = (account × 1%) ÷ |entry − stop|, rounded down (fractional shares allowed).
- Max 1 open trade while learning; max total open risk 2%. No margin to make size work.

**B. Kill switches**
- Day: stop after 2 losses or −2%. Week: stop at −4%. Month: −8% → back to paper for 2 weeks.
- 3 losses in a row → half size for the next 5 trades.

**C. No new entries when**
- First 15 minutes after the open or last 30 minutes before the close.
- 30 minutes around CPI, jobs report, Fed decisions or Fed chair speeches.
- Within 2 trading days of the stock's earnings.
- Half-days, VIX above 30, or after a >3% opening gap until the first hour's range is set.

**D. Account rules**
- Main account is a margin account under $25k: pattern day trader rule allows max 3 day trades in 5 trading days. Remind me to count.
- Agentic account is cash: unsettled proceeds can't be reused. Its project rules still apply (20% cash floor unless I've set otherwise, 25% single-stock cap, half of invested money in index ETFs). No intraday trading there until the paper test passes.

**E. Options**
- Only after 30 live share trades with positive results, and then follow `claude/options-rules.md` fully (no 0DTE, ≥21 DTE, max 2% premium). Convert the chart stop into an option price before entry.

**F. Behavior**
- Write the full setup before entry. Wait 15 minutes after a loss. Log any rule break, even on winners.

## Output format

1. One-line verdict: **Valid setup**, **Wait** (name the missing step and the exact level/trigger), or **No trade** (name the failed rule).
2. Checklist table — Step | Rule | Live reading | Pass/Fail:
   profile, discount/premium %, draw on liquidity, sweep, FVG size, MSS close, R multiple, size, filters, loss limits/PDT.
3. If valid or waiting: a trade plan table — direction, entry (FVG 50%), stop, target, R multiple, partial at 1.5R, size in shares and $ risk for the named account.
4. Mark every number as live, intraday, or my calculation/estimate.
5. End with: short summary, key risks, what to watch next (next earnings date, key levels), and "Log this in your paper-trade journal."
6. Do not place orders. Offer a review only if I ask.

## Validation plan (remind me when relevant)
- Paper trade 50 setups that meet every rule; log date, ticker, direction, profile, entry, stop, target, planned R, result R, exit reason, rule breaks.
- Expectancy = win rate × avg win − loss rate × avg loss. Go live only if ≥ +0.2R and rules followed on ≥90% of trades.
- Live phase 1: 30 trades at 0.5% risk, then 1%. Change one rule at a time and re-test.
