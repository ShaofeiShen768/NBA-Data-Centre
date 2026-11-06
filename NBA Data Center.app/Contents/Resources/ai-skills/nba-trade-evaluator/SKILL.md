---
name: nba-trade-evaluator
description: Build and evaluate NBA trades with real rosters, contracts, salary-cap levels and future draft picks; legality comes only from the deterministic CBA rules engine; can simulate the other team's GM and propose counteroffers.
version: 1.0.0
category: trade
allowed-tools: [search_players, search_teams, get_team_roster, get_player_contracts, get_team_contracts, get_team_salary_cap, get_team_salary_structure, get_team_draft_picks, get_player_advanced_stats, validate_nba_trade, evaluate_trade, negotiate_trade, suggest_trade_improvements, save_trade_simulation]
keywords: [交易, 模拟交易, 换, 送出, 得到, 报价, 反报价, 总经理, 选秀权, 薪资匹配, 土豪线, trade, deal, counteroffer, offer, GM, swap, salary matching, apron]
requires-tool-calling: true
---

# NBA Trade Evaluator

## Objective
Produce a trade analysis whose legality verdict comes exclusively from `validate_nba_trade`, with value analysis and an optional simulated GM response.

## When to Use
Any request to construct, check, grade, negotiate or improve a trade between 2–4 teams.

## Required Inputs
Teams (IDs), trade date (YYYY-MM-DD, ET; default today), season (start year; current season 2026), assets: players (player_id), picks (pick_id from `get_team_draft_picks`), cash.

## Workflow
1. Resolve teams; fetch `get_team_roster` and `get_team_salary_structure` for each.
2. Fetch picks with `get_team_draft_picks`; only use pick_id values returned there. Frozen picks are not tradable; "pool/uncertain" picks need confirmation.
3. Build a proposal JSON `{season, trade_date, teams, assets:[{kind, from_team, to_team, player_id|pick_id|amount}]}`.
4. Call `validate_nba_trade`. Report legality exactly as returned: legal / illegal / unknown (missing information).
5. Call `evaluate_trade` for grades (simulation model, not probabilities).
6. If asked for a GM response, call `negotiate_trade` to get context and answer as the other GM, clearly labelled as simulation.
7. Any modified proposal (counteroffer, improvement) must be validated again with `validate_nba_trade` before presenting it.
8. Save with `save_trade_simulation` only if the user asks to save.

## Available Tools
See `allowed-tools`. `save_trade_simulation` writes only to the simulation tables.

## Constraints
- Never declare a trade legal yourself. If validation cannot run, say "unknown".
- Never invent players, contracts or picks; use only IDs returned by tools.
- Cap hit, base salary and cash are different numbers; salary matching uses cap hit.

## Output Schema
Sections: **交易合法性 / Trade legality** (engine verdict + failed/unknown rules), **球队评分 / Team ratings** (both teams + balance), **财务影响 / Financial impact** (before/after salary, apron status), **篮球影响 / Basketball impact**, **资产估值 / Asset valuation** (players, picks with protections), **模拟总经理回应 / Simulated GM response**, **建议反报价 / Suggested counteroffer** (with its own validation result), **数据来源 / Data sources** (contracts sync time, cap source, pick file date).

## Error Handling
Missing contract data → report which player lacks it and mark legality unknown. Nonexistent pick → the engine fails it; report and suggest real picks.
