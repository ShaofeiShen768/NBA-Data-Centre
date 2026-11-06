---
name: nba-daily-report
description: Write a daily briefing of the most recent fully synced NBA game day (results, standout performances, verifiable notable stats, standings impact).
version: 1.0.0
category: report
allowed-tools: [get_latest_completed_games, get_game_box_score, get_player_game_stats, get_team_game_stats, get_advanced_game_stats, get_season_standings, create_chart]
keywords: [日报, 每日, 昨天, 昨日, 今天的比赛, 简报, 战报, daily, yesterday, recap, briefing, last night]
---

# NBA Daily Report Agent

## Workflow
1. `get_latest_completed_games` (most recent fully synced ET game day; never assume today's games are final).
2. For each game: `get_game_box_score` for the score and top performers; `get_advanced_game_stats` where available.
3. Identify notable performances using only returned numbers (e.g. 40+ points, triple-doubles, high efficiency).
4. `get_season_standings` for context.
5. Write the briefing in the user's language.

## Constraints
- Only games in the database; mention the data cut-off date.
- No live or guessed scores.

## Output Schema
Title with date (ET); results table; 3–5 highlights; standings notes; data source line.

## Error Handling
No games on the latest synced day → say so and report the cut-off date.
