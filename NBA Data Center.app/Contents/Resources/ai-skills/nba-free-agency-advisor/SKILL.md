---
name: nba-free-agency-advisor
description: Analyze current and upcoming NBA free agents (UFA/RFA/player option/team option) and recommend signing targets for a team with rule-checked feasibility.
version: 1.0.0
category: free-agency
allowed-tools: [search_players, search_teams, get_current_free_agents, get_future_free_agents, get_player_contract, get_player_contracts, get_team_salary_cap, get_team_salary_structure, get_team_roster, get_player_advanced_stats, evaluate_free_agent_fit]
keywords: [自由球员, 自由市场, 签约, 续约, 球员选项, 球队选项, 受限, 非受限, free agent, free agency, sign, UFA, RFA, player option, team option]
---

# NBA Free Agency Advisor

## Workflow
1. Resolve the team; fetch `get_team_roster` and `get_team_salary_structure`.
2. `get_current_free_agents` (status: unsigned_confirmed / needs_verification / signed) or `get_future_free_agents` (fa_year, category).
3. Identify needs from the roster (positions, age, contracts ending).
4. Shortlist candidates with stats (`get_player_advanced_stats`).
5. For each recommended signing call `evaluate_free_agent_fit` with a proposed first-year salary, years and raise.

## Constraints
- An expiring contract does not prove a player is unsigned; report the status and its evidence.
- Unexercised options: say the player may not reach free agency.
- Feasibility comes from `evaluate_free_agent_fit`: clear / conditional / not possible / unverifiable.

## Output Schema
Team needs; candidate table (status, age, position, stats, last contract); recommendations with feasibility and mechanism; uncertainties; sources.

## Error Handling
Future cap not published → feasibility "unverifiable"; say so.
