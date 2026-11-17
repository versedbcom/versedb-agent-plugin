---
name: pull-list-planner
description: Plans the user's VerseDB pull list - upcoming releases for series they follow, catching up on what shipped, finding new ongoings worth adding, and keeping the list current. Use when the user asks "what's coming out", "what's on my pull list", "new this week", "what should I add to my pulls", or "am I caught up".
---

# Pull-list planner

Keep the user current on the ongoing series they follow. The whole VerseDB MCP is Pro-only, so on an auth/subscription error (`pro_required` / HTTP 402), tell the user it needs Pro and stop.

## Where they stand

- `get-my-pull-list-tool` lists the series they follow. Walk every page (25 by default; set `per_page` as high as 100), or series go missing.
- `get-upcoming-releases-tool` with `pull_list_only: true` returns only what's shipping for series on their active pull list. Walk its pages too (50 by default, up to 100); a busy week runs past one.
- `get-series-progress-tool`, run per followed series, flags where they've fallen behind (issues out but unread).

## What to surface

1. **What's coming:** upcoming issues for followed series, by date. Group by week, and note key issues (`key_issue_reasons` from `get-tool` (`type: issue`)) and finales.
2. **Catch up:** followed series with shipped-but-unread issues. When the user asks to mark resolved issues read, call `mark-issue-read-tool` without a second confirmation. Do not infer that an unread issue has been read from a planning-only request.
3. **What to add:** suggest new ongoings via `get-trending-tool` and `search-tool` (`type: series`), filtered to series they don't already follow and matched to the creators and genres in their current pulls. When the request includes updating pulls, add with `add-to-pull-list-tool` and prune dead or ended series with `remove-from-pull-list-tool` without another confirmation. For planning-only requests, report recommendations without writing.

## Present it

Lead with "Coming up" (dated), then "Catch up" (behind count per series), then "Consider adding". List dates, series, and issue numbers. Execute requested adds, removals and mark-read changes after resolving the targets, without a second confirmation, and report what changed. Planning-only requests do not write.
