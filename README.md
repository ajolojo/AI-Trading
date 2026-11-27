# AI-Trading

Claude skills for a small, rules-based trading workflow: check why a stock moved, decide whether the business or just the price changed, size a small starter position, and grade intraday entries with a liquidity sweep model.

> ## ⚠️ Read this first
> - **Not financial advice.** These skills produce analysis only. You make every decision and you are responsible for every trade.
> - **Untested.** The liquidity sweep model is in its **paper-trading phase**. It has not been backtested or proven profitable.
> - **Use at your own risk.** Markets, options and intraday trading can lose money fast, including more than you planned.
> - **The skills never place orders on their own.** Any order must be reviewed and then confirmed by you with an exact phrase (see [Safety rules](#safety-rules)).

---

## Skills

| Skill | Use it for | Example prompt |
|---|---|---|
| [`stock-checkup`](skills/stock-checkup/SKILL.md) | "What happened to X / should I buy X?" Finds what moved it, compares the move to SPY and a sector ETF, checks the latest quarters, looks at your combined exposure, and sizes a small starter position. | "Look at NOW. Why is it down and is it worth buying?" |
| [`liquidity-sweep-model`](skills/liquidity-sweep-model/SKILL.md) | Entry timing. Grades a setup step by step (market profile → discount/premium → draw on liquidity → sweep → fair value gap → market structure shift) and returns **Valid setup**, **Wait**, or **No trade**, with a trade plan and risk checks. | "Check SPY with my sweep model." |

They work together: `stock-checkup` answers *whether* to own something, then offers to run `liquidity-sweep-model` for *when* to enter.

---

## Requirements

- **Claude** with skills enabled (claude.ai, the desktop app, or Claude Code).
- **Robinhood connector**, connected and signed in. The skills read quotes, price history, fundamentals, earnings dates, positions and balances through it.
  - Without it, `stock-checkup` can still do the web research parts (news, earnings, analyst changes) and will label them as web-only. `liquidity-sweep-model` needs live price data and will stop.
- **Web search** enabled in Claude, for news and the cause of a move.

---

## Install

**Claude (claude.ai or desktop)**
1. Download this repo (Code → Download ZIP) and unzip it.
2. Zip one skill folder on its own, for example `skills/stock-checkup`.
3. In Claude, upload the zip as a skill (in Settings, under skills).
4. Repeat for `skills/liquidity-sweep-model`.

**Claude Code**
- Copy the folders in `skills/` into `~/.claude/skills/` (all your projects) or `.claude/skills/` (one project).

---

## Customize before use

The skills were written for one person's accounts and rules. Edit these before you rely on them. Each skill is a plain-text `SKILL.md` you can change.

| Setting | Where | Default | What to change |
|---|---|---|---|
| Tradable account | Both skills, "Setup" / "Connector and context" | A Robinhood account nicknamed `MCP` | Your own account's nickname. Keep one account as the only one the skills may suggest trades for. |
| Read-only account | Both skills | "main account" | Any other accounts you want treated as context only. |
| Cash floor | `stock-checkup` step 4 | Keep 20% of account value in cash after a buy | Your floor, as a % or a fixed dollar amount. |
| Single-stock cap | `stock-checkup` step 4 | No stock above 25% of the account | Your cap. |
| Index ETF share | `stock-checkup` step 4 | At least half of invested money in broad index ETFs (VTI, VXUS) | Your rule and your preferred ETFs. |
| Risk per trade | `liquidity-sweep-model` Guardrails A | 1% of account value | Your risk %. |
| Loss limits | `liquidity-sweep-model` Guardrails B | Stop after 2 losses or −2% a day, −4% a week, −8% a month | Your limits. |
| Day-trade limit | `liquidity-sweep-model` Guardrails D | Pattern day trader rule: 3 day trades in 5 days for margin accounts under $25,000 | Remove if your account is over $25,000 or a cash account. |
| Options | Both skills | No options in the tradable account | Only change this if you have your own written options rules. |

### Optional rule documents

The skills mention two documents: `claude/trading-model.md` (full sweep-model rules) and `claude/options-rules.md` (options rules). **You don't need them.** If they aren't there, the skills use the rules written inside each `SKILL.md`. If you add your own, put them in a Claude Project and the skills will follow them, and tell you where they differ.

---

## Safety rules

Built into both skills:

- **No automatic orders.** The skills analyze only. If you ask for an order, Claude first runs Robinhood's order review and shows you the price, cost and any warnings, then **stops**.
- **Exact confirmation phrase.** An order is placed only when you reply with the exact words `PLACE THE ORDERS`. Anything else means no.
- **One tradable account.** Every other account is read-only.
- **Live data only.** Every number is labeled live, intraday, after-hours, official close, or estimate. No numbers from memory.
- **Start small, buy in pieces**, and write down why you're buying, the price where you're wrong, and a profit target before any buy.
- **Sweep model kill switches:** stop trading for the day after 2 losses, and pause after weekly or monthly loss limits.

---

## Paper trade first

Before using the sweep model with real money:

1. Paper trade **50 setups** that meet every rule.
2. Log each one: date, ticker, direction, entry, stop, target, planned R, result in R, exit reason, any rule breaks.
3. Calculate **expectancy** = win rate × average win − loss rate × average loss (in R).
4. Go live only if expectancy is at least **+0.2R** and you followed the rules on at least 90% of trades.
5. Start live at half size (0.5% risk) for 30 trades, then re-check.

---

## Status

| Skill | Version | Status |
|---|---|---|
| `stock-checkup` | v1 | In use for research |
| `liquidity-sweep-model` | v1 | Paper-trading phase, not validated |

---

## License

[MIT](LICENSE). You're free to use, copy, change and share these skills, as long as the license notice stays with them. The software is provided "as is", without warranty, and the authors aren't liable for any losses. That includes trading losses: see [Read this first](#️-read-this-first).
