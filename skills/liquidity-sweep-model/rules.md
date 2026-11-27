# My rules (edit this file)

Your personal settings for the `liquidity-sweep-model` skill. **Fill in the "Your setting" column with your own numbers.** The "Example" column shows the repo owner's settings for reference only. The skill never uses the example column.

If a required setting is blank, Claude asks you for it before grading and suggests you save it here. It won't assume the example values.

The structural safety rules in `SKILL.md` (the "Safety core": paper-trading only, exact account match, no borrowing, fail-closed) always apply and can't be turned off here.

## Account

| Setting | Your setting | Example | Notes |
|---|---|---|---|
| Account the plan is sized for | `` | `MyTradingAcct` | Must match the broker nickname exactly. |
| Account type | `` | cash | `cash` or `margin`. |
| Day-trade rule your broker applies | `` | Ask broker | FINRA replaced the "pattern day trader" rule and its $25,000 minimum with intraday margin standards effective June 4, 2026; brokers can phase it in until October 20, 2027. Write what your broker currently applies. |
| Cash floor | `` | Same as `stock-checkup` (20%) | Cash that must remain; size is capped at settled cash minus this. |

## Your risk settings (required)

| Setting | Your setting | Example | Notes |
|---|---|---|---|
| Risk per trade | `` | 1% of account value | The most you'll lose if the stop is hit. |
| Max open trades | `` | 1 while learning | |
| Total open risk | `` | 2% of account value | Across all open trades. |
| Single-stock cap | `` | 25% of account value | Max position size per stock. |
| Daily loss stop | `` | 2 losing trades or −2% | Stop trading for the day. |
| Weekly loss stop | `` | −4% | |
| Monthly loss stop | `` | −8%, then 2 weeks paper only | |
| Losing-streak rule | `` | After 3 losses in a row, half size for 5 trades | |

Claude warns you (once per conversation) if a setting looks aggressive: risk per trade over 2%, total open risk over 6%, or no daily loss stop. It still uses your setting.

The skill asks for your recent results (from your journal) before grading, so it can check the loss stops.

## No new entries when

These are part of the model. You can add more.

- First 15 minutes after the open, or the last 30 minutes before the close.
- 30 minutes before and after CPI, the jobs report, Fed decisions, or Fed chair speeches.
- Within 2 trading days of the stock's earnings.
- Half-days, VIX above 30, or after an opening gap above 3% until the first hour's range is set.

## Validation targets

| Setting | Your setting | Example |
|---|---|---|
| Paper setups before going live | `` | 50 |
| Minimum expectancy to go live | `` | +0.2R |
| Rule-follow rate | `` | At least 90% of trades |
| First live phase | `` | 30 trades at half your risk per trade |
