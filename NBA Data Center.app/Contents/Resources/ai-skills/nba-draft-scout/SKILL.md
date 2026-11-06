---
name: nba-draft-scout
description: Analyze NBA draft prospects (2027 and later) from multi-source consensus rankings, ranking history and available stats; team-need fits and mock-draft reasoning with traceable sources.
version: 1.0.0
category: draft
allowed-tools: [get_draft_prospects, get_mock_draft_rankings, get_prospect_rankings, get_prospect_stats, get_prospect_ranking_history, get_prospect_scouting_report, get_team_draft_picks, get_team_roster, search_teams]
keywords: [选秀, 新秀, 状元, 乐透, 模拟选秀, 球探, 顺位, 大学, draft, prospect, mock draft, big board, lottery, scouting, rookie]
---

# NBA Draft Scouting Agent

## Objective
Prospect analysis grounded in ranking facts with source names, dates and links.

## When to Use
Questions about upcoming draft prospects, rankings, risers/fallers, team fits, mock drafts.

## Required Inputs
Draft year (default 2027), optional team.

## Workflow
1. `get_draft_prospects` for the board (consensus rank, average of ranking sources, best/worst, change).
2. For named prospects: `get_prospect_rankings` (per-source picks with links and dates) and `get_prospect_ranking_history`.
3. `get_prospect_stats`; if empty, say stats are not available locally (college/international stats are not NBA stats).
4. For team fit: `get_team_roster` and `get_team_draft_picks` (real pick ownership).
5. Write the report separating facts / scouting consensus / AI inference.

## Constraints
- Never invent measurements, stats or quotes; never reproduce paid scouting text.
- Hypothetical pick numbers in mock drafts are not real pick ownership.

## Output Schema
Per prospect: facts (with source), consensus position, ranking movement, AI inferences (labelled), risks, team fits; then a source list with dates.

## Error Handling
Prospect not found → list closest names from the board.
