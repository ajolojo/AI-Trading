# My rules (edit this file)

Personal settings for the `liquidity-sweep-model` skill. Edit the values on the right.

You can make these rules **stricter** at any time. You can't make them looser than the hard limits in `SKILL.md` (the "Safety core"). If you set something looser, the skill uses the hard limit and tells you.

## Account and phase

| Setting | Value | Notes |
|---|---|---|
| Account the plan is sized for | `` | Nickname of the account to size paper trades against. Leave blank to be asked. Must match exactly; the skill shows its last 4 digits. |
| Phase | Paper trading | Hard limit in this version: the skill never reviews or places orders. |

## Risk and size

| Setting | Default | Hard limit |
|---|---|---|
| Risk per trade | 1% of account value | 1% max |
| Max open trades while learning | 1 | — |
| Max total open risk | 2% of account value | 2% max |
| Size cap | Settled cash minus your cash floor, and 25% of account value per stock | Always applies |
| Minimum stop distance | 0.1% of price | Tighter stops = "No trade" |

## Kill switches

| Setting | Default |
|---|---|
| Daily stop | After 2 losing trades or −2% |
| Weekly stop | −4% |
| Monthly stop | −8%, then 2 weeks of paper trading only |
| Losing streak | After 3 losses in a row, half size for the next 5 trades |

The skill asks you for your recent results (from your journal) before grading, so it can check these.

## No new entries when

- First 15 minutes after the open, or the last 30 minutes before the close.
- 30 minutes before and after CPI, the jobs report, Fed decisions, or Fed chair speeches.
- Within 2 trading days of the stock's earnings.
- Half-days, VIX above 30, or after an opening gap above 3% until the first hour's range is set.

## Account type notes

| Setting | Value | Notes |
|---|---|---|
| Account type | `` | `cash` or `margin`. Leave blank to be asked. |
| Day-trade limit | Ask my broker | FINRA replaced the "pattern day trader" rule and its $25,000 minimum with new intraday margin standards effective June 4, 2026, but brokers can phase the change in until October 20, 2027. Check which rule your broker applies and write it here. |
| Cash accounts | Settled cash only | Using unsettled sale proceeds can cause good-faith or free-riding violations. |

## Validation targets

| Setting | Default |
|---|---|
| Paper setups before going live | 50 |
| Minimum expectancy to go live | +0.2R |
| Rule-follow rate | At least 90% of trades |
| First live phase | 30 trades at 0.5% risk, then 1% |
