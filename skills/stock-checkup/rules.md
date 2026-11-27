# My rules (edit this file)

This file holds your personal settings for the `stock-checkup` skill. Edit the values on the right. Everything else in the skill stays the same.

You can make these rules **stricter** at any time. You can't make them looser than the hard limits in `SKILL.md` (the "Safety core"). If you set something looser, the skill uses the hard limit and tells you.

## Accounts

| Setting | Value | Notes |
|---|---|---|
| Tradable account nickname | `` | Leave blank to be asked on first use. The skill only suggests trades for the one account whose nickname matches **exactly**, and always shows its last 4 digits. |
| Other accounts | read-only | Used only to check what you already own. Never traded. |

## Position sizing (for the tradable account)

| Setting | Default | Notes |
|---|---|---|
| Core holding | VTI | Broad US index ETF used to balance single-stock buys. |
| International holding | VXUS | Used when you already hold mostly US stocks. |
| Cash floor | 20% of account value | Cash that must remain after any buy, measured in **settled** cash. You may use a dollar amount instead, e.g. `$200`. |
| Single-stock cap | 25% of account value | Maximum in any one stock after the buy. Hard limit: 25%. |
| Index ETF share | At least 50% of invested money | Pair a stock buy with an equal index ETF buy when needed. |
| Buy in pieces | Yes: first piece now, the rest on a named trigger | Never the full position at once. |
| Earnings flag | 14 days | Flag any holding or candidate that reports within this window before buying. |
| Options | Not allowed | Hard limit. The skill gives general education only. |

## Before any buy, write down

1. **Why** you're buying (one sentence).
2. **Add more if:** a specific price or event.
3. **Wrong if:** a specific closing price.
4. **Profit target:** a specific level.

## What the checkup covers

When you ask about a stock, the skill works through these checks:

1. **What moved it?** The specific event (earnings, guidance, analyst change, news, SEC filing, sector move, or macro such as Fed or jobs data), its date, and the size of the move. If there's no clear cause, it says so instead of guessing.
2. **Business or just price?** Latest quarter vs prior quarters (revenue, gross and operating margins, guidance), and the move vs the S&P 500 and the sector ETF. Verdict: "the business changed" (be cautious) or "broad selloff / overreaction" (possible opportunity).
3. **Start small.** A small starter position, what would make you add more, and what would tell you you're wrong.

It ends with a short summary, the key risks, and what to watch next (like the next earnings date).
