---
name: collection-insights
description: Analyzes the user's VerseDB comic collection - total and per-issue market value, run gaps (missing issues in series they own), reading progress, and which key issues they hold. Use when the user asks "what's my collection worth", "what am I missing", "analyze my collection", "what should I read next from what I own", or "what are my most valuable books".
---

# Collection insights

Turn a user's collection into something they can act on: what it's worth, where the runs have gaps, what to read next. The whole VerseDB MCP is Pro-only, so if any call (e.g. `get-my-collection-tool`) returns an auth/subscription error (`pro_required` / HTTP 402), tell the user it needs Pro and stop.

## Pull the collection

Start with `get-my-collection-tool`. Its `summary` covers the whole collection, so you don't need every page for the totals. Walk every page (25 by default; set `per_page` as high as 100) before listing books, or the per-book sections come out short.

## Value

- `summary.total_estimated_value` adds up the values the user recorded on their copies, the same figure the site shows as Market Value. `copies_with_estimated_value` says how many copies that covers. Say so plainly: it's their number, not an appraisal.
- For copies with no recorded value (or the notable ones), call `get-market-prices-tool`. Prices are **grade-dependent**: match each value to the grade/condition recorded on the collection item, and state that grade. Never quote a bare number.
- Report total estimated value, the top N most valuable books, and any notable movers (the tool exposes recent sales and trends). Flag that values are estimates with sale dates, not guarantees.

## Gaps

For series the user collects:
- `get-series-issues-tool` lists the full run; diff against owned issues to find missing numbers.
- Use `get-series-progress-tool` for reading completion alongside ownership completion ("you own 18/22, have read 12").
- Surface the cheapest gaps to close first (cross-reference `get-market-prices-tool`).

## Key issues held

Call `get-tool` (`type: issue`) on owned books and read `key_issue_reasons` to highlight first appearances, origins, deaths, and other significance the user may not realize they own.

## Present it

Lead with the headline (total value, # of series, % read), then sections: Most valuable · Run gaps · Key issues · Suggested next read. Offer to save a "missing issues" list (`list-tool`: `create`, then `add_item`) or add the gaps to their pull list.

The server ships a `collection-analysis-prompt` prompt; prefer it when the user wants a broad overview rather than a specific cut.
