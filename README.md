# AI-Trading

Claude skills for my rules-based trading workflow. Analysis, not advice.

## Skills

| Skill | What it does |
|---|---|
| [`liquidity-sweep-model`](skills/liquidity-sweep-model/SKILL.md) | Grades a stock or ETF setup with my liquidity sweep model (sweep → FVG → market structure shift) using live Robinhood data, with 1% risk sizing, kill switches, news/earnings filters and pattern-day-trader checks. Never places orders. |

## Install

- **Claude (claude.ai / desktop):** zip the `skills/liquidity-sweep-model` folder and upload it under Settings → Capabilities → Skills.
- **Claude Code:** copy the folder into `~/.claude/skills/` (personal) or `.claude/skills/` in a project.

## Requirements

- Robinhood connector, signed in.
- Optional project docs: `claude/trading-model.md` (takes precedence over the skill if they differ) and `claude/options-rules.md`.

## Status

v1 — paper-trading phase. Go live only after 50 paper setups show expectancy of at least +0.2R.
