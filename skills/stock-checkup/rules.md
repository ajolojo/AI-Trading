# My rules (edit this file)

Your personal settings for the `stock-checkup` skill. **Fill in the "Your setting" column with your own numbers.** The "Example" column shows the repo owner's settings for reference only. The skill never uses the example column.

If a required setting is blank, Claude asks you for it before sizing anything and suggests you save it here. It won't assume the example values.

The structural safety rules in `SKILL.md` (the "Safety core": order confirmation, exact account match, no borrowing, fail-closed) always apply and can't be turned off here.

## Accounts

| Setting | Your setting | Example | Notes |
|---|---|---|---|
| Tradable account nickname | `` | `MyTradingAcct` | Must match the broker nickname exactly. Claude shows its last 4 digits before any order. |
| Other accounts | read-only | read-only | Used only to check what you already own. Never traded. |

## Your risk settings (required)

| Setting | Your setting | Example | Notes |
|---|---|---|---|
| Cash floor (cash left after any buy) | `` | 20% of account value | A % of account value or a $ amount, e.g. `$200`. Measured in settled cash. |
| Single-stock cap | `` | 25% of account value | Most you'll hold in any one stock after a buy. |
| Index ETF share of invested money | `` | At least 50% | Set `0%` if you don't want this rule. |
| Core index ETF | `` | VTI | Used to balance single-stock buys. |
| International ETF | `` | VXUS | Used when you hold mostly US stocks. |
| Buy in pieces | `` | Yes: first piece now, the rest on a named trigger | `Yes` or `No`. |
| Earnings flag window | `` | 14 days | Flags a stock that reports within this many days. |

Claude warns you (once per conversation) if a setting looks aggressive: cash floor under 10%, single-stock cap over 50%, or index share under 25%. It still uses your setting.

## Options

| Setting | Value | Notes |
|---|---|---|
| Options in the tradable account | Not supported | This skill gives general education only. |

## Before any buy, write down

1. **Why** you're buying (one sentence).
2. **Add more if:** a specific price or event.
3. **Wrong if:** a specific closing price.
4. **Profit target:** a specific level.

## What the checkup covers

1. **What moved it?** The specific event (earnings, guidance, analyst change, news, SEC filing, sector move, or macro such as Fed or jobs data), its date, and the size of the move. If there's no clear cause, it says so.
2. **Business or just price?** Latest quarter vs prior quarters (revenue, gross and operating margins, guidance), and the move vs the S&P 500 and the sector ETF. Verdict: "the business changed" (be cautious) or "broad selloff / overreaction" (possible opportunity).
3. **Start small.** A small starter position sized to your settings above, what would make you add more, and what would tell you you're wrong.

It ends with a short summary, the key risks, and what to watch next (like the next earnings date).
