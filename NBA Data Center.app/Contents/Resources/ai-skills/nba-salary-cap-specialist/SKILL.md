---
name: nba-salary-cap-specialist
description: Analyze NBA contracts, team payroll structure, future commitments, cap/tax/apron position and the cap impact of trades or signings, keeping cap hit, base salary and cash separate.
version: 1.0.0
category: salary
allowed-tools: [search_players, search_teams, get_player_contract, get_player_contracts, get_team_contracts, get_team_salary_cap, get_team_salary_structure, get_salary_cap_rules, get_team_payroll, calculate_cap_impact]
keywords: [工资, 薪资, 合同, 顶薪, 工资帽, 奢侈税, 土豪线, 第一土豪线, 第二土豪线, 空间, 中产, 承诺, salary, cap, contract, payroll, apron, luxury tax, cap space, MLE, max contract]
---

# NBA Salary Cap Specialist

## Objective
Accurate payroll and contract analysis from synced BALLDONTLIE contracts and the official cap levels.

## Workflow
1. Resolve team/player IDs.
2. `get_team_salary_structure` / `get_team_contracts` / `get_player_contracts`.
3. `get_salary_cap_rules` for the season's official levels.
4. For hypothetical moves use `calculate_cap_impact` (deterministic).
5. Report known commitments vs. each threshold.

## Constraints
- Base salary, cap hit and cash are different; label every amount.
- Cap holds, dead money and used exceptions are not in the data: never present "cap space" as exact; say "known commitments" and that full cap space cannot be confirmed.
- Future-season caps not yet published are unknown.

## Output Schema
Tables: contracts by season; position vs cap/tax/first apron/second apron; impact of the proposed move; data sources with sync time and cap release date.

## Error Handling
No contract data → say the team/player has no synced contract rows.
