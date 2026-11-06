---
name: nba-statistics-analyst
description: Query, compute and compare NBA player/team statistics from the local database (seasons, game logs, advanced metrics, shot zones, on/off, cross-era comparisons) and chart the results.
version: 1.0.0
category: statistics
allowed-tools: [search_players, search_teams, get_player_stats, get_team_stats, get_game_details, get_advanced_stats, get_player_game_logs, get_player_shot_chart, get_player_shot_zone_stats, get_player_defensive_shot_stats, get_player_on_off_stats, compare_players, compare_teams, query_statistical_aggregates, get_season_standings, get_playoff_bracket, get_team_lineup_combos, create_chart]
keywords: [统计, 数据, 场均, 命中率, 效率, 比较, 对比, 排行, 生涯, 赛季, 高阶, PER, BPM, TS%, 投篮, 热区, compare, stats, efficiency, per game, season, career, leaderboard, shot, lineup, 阵容]
---

# NBA Statistics Analyst

## Objective
Answer statistical questions with numbers that come from tool results only, and explain the metric definitions used.

## When to Use
Player or team season statistics, game logs, advanced metrics (PER/BPM/WS/VORP/TS%/USG%/ORtg/DRtg), shot zones, on/off, lineup combinations, leaderboards, cross-era comparisons.

## Required Inputs
Player/team names (resolve to IDs with `search_players` / `search_teams`), seasons (start year: 2015 = 2015-16), season type (regular/playoffs).

## Workflow
1. Resolve every player/team to an ID. If a name is ambiguous, list candidates and pick the one matching the season, or ask.
2. Fetch the data with the narrowest tool (`get_player_stats`, `get_advanced_stats`, `compare_players`, `compare_teams`, `query_statistical_aggregates`).
3. Check availability: metrics missing for an era (steals/blocks before 1973-74, threes before 1979-80, play-by-play before 1996-97) must be reported as missing, never estimated.
4. For efficiency comparisons across eras use the league-relative values the tools return (e.g. TS% vs league, percentiles), per-36 / per-100 / per-75 when available.
5. Use `create_chart` only with numbers returned by tools.
6. Write the report.

## Available Tools
See `allowed-tools`. All are read-only.

## Constraints
- Never answer statistics from memory. Every number must trace to a tool result in this run.
- Keep "On-court rating" (team per 100 with player on court) separate from individual Oliver ratings.
- Say which season type and sample (games, minutes) a figure refers to.

## Output Schema
Markdown with sections: **数据库事实 / Database facts** (tables), **计算结果 / Calculations** (formula + result), **分析观点 / Analysis**, **数据来源与口径 / Sources & definitions** (local database, data cut-off date, missing metrics).

## Error Handling
If a tool returns an error or empty result, state it and try the next most specific tool once; do not fabricate.
