---
name: stock-checkup
description: Check a stock I ask about: what moved it, whether the business or just the price changed, and a small rules-based starter position, using live Robinhood data.
---

# Stock Checkup

Use this skill whenever I ask about a specific stock or ETF: "look at X", "why is X down/up", "should I buy X", "is X a good entry", "what happened to X". For intraday entry timing (sweeps, FVGs, structure shifts), hand off to the `liquidity-sweep-model` skill after this checkup.

Analysis, not advice. I make every decision. Never place an order from this skill. If I ask for an order, follow the project ORDERS rules: review with the connector, show it, STOP, place only on the exact words PLACE THE ORDERS, agentic account only.

## 0. Setup
- Load Robinhood tools with ToolSearch. Call get_accounts. If the connector isn't signed in, say so and do the web-only parts, clearly labeled.
- Accounts: agentic (Robinhood nickname "MCP") is the only tradable account; the main account is read-only context.
- Read the project rules (agentic account rules) if available; they override the defaults below.
- Label every number: live, intraday, after-hours, official close, or my calculation/estimate.

## 1. What moved it?
- Pull: get_equity_quotes (ticker, SPY, sector ETF), get_equity_historicals (daily, ~3 months) to find the move's start date and size.
- Search the web for the cause. Check, in order: earnings or guidance, analyst rating or target changes (get_equity_analyst_ratings), company news, SEC filings (8-K etc.), sector move, macro (Fed, CPI, jobs).
- Report: the event, its date, and the size of the move in % and $. If there's no clear cause, say "no clear cause found" instead of guessing.

## 2. Business or just price? (always for drops; brief for rises)
- **Fundamentals:** latest quarter vs the prior 3 quarters and the same quarter last year, using get_financials / get_earnings_results / filings: revenue and growth, gross margin, operating margin, EPS vs estimate, guidance (raised, held, cut). Show as a small table.
- **Market vs sector:** compare the move over the same day and week to SPY and the sector ETF:

| Sector | ETF |
|---|---|
| Semiconductors | SMH |
| Software | IGV |
| Broad tech | XLK |
| Communication / internet | XLC |
| Consumer discretionary | XLY |
| Consumer staples | XLP |
| Energy | XLE |
| Financials | XLF |
| Healthcare | XLV |
| Biotech | XBI |
| Industrials / defense | XLI (ITA for defense) |
| Utilities | XLU |
| Real estate | XLRE |
| Materials | XLB |
| Small caps | IWM |
| International | VXUS |

- **Technical context:** price vs 50-day and 200-day averages, RSI, 52-week high/low (get_equity_technical_indicators, get_equity_fundamentals).
- **Verdict, one of:**
  - "The business changed" (be cautious): revenue, margins or guidance got worse.
  - "Broad selloff / overreaction" (possible opportunity): fundamentals intact and the market or sector fell too.
  - "Mixed": say which parts point each way.

## 3. Combined exposure
- Pull positions from both accounts (get_equity_positions, get_option_positions, get_portfolio).
- Show what I already own in this stock or sector, including rough look-through via VTI or other ETFs (label as estimate).
- Flag if a buy would add to an existing concentration (for example AI/semiconductors). Prefer ideas that balance it.

## 4. Position sizing: start small
Apply the agentic account rules (defaults if the project doesn't say otherwise):
- Cash floor after the buy (project setting; default 20% of account value).
- No single stock above 25% of the account.
- At least half of invested money in broad index ETFs; if needed, pair the stock with an equal VTI (or VXUS) buy.
- Buy in pieces: a first piece now, the rest on a named trigger.
- Flag earnings within the next 2 weeks before suggesting a buy.
- No options in the agentic account. If I ask about options, use `claude/options-rules.md`.

Write the pre-buy notes:
- **Why** I'm buying (one sentence).
- **Add more if:** a specific price or event (for example reclaims the 50-day average, or holds a level after a catalyst).
- **Wrong if:** a specific closing price (support level, 52-week low, or gap fill).
- **Profit target:** a specific level (prior support/resistance, moving average, or analyst-target zone).

Show the starter size in dollars and as % of the account, and confirm each rule passes.

## 5. Output format
1. **Headline:** price, move, and the cause in one or two lines.
2. **What moved it:** event, date, size.
3. **Business or price:** fundamentals table, move vs SPY and sector ETF, technical context, verdict.
4. **Your exposure:** both accounts.
5. **Starter plan:** size, rule checks, why / add if / wrong if / target.
6. **Summary, key risks, what to watch next** (next earnings date, key levels, macro dates).
7. **Sources** as markdown links for anything from the web.

Keep it scannable: tables for numbers, short sentences. Offer, in one line, to review an order or run the `liquidity-sweep-model` check for entry timing.
