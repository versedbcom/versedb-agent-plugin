---
name: reading-order
description: Build a chronological reading order for a comic character, story arc, event, creator run, or series using the VerseDB MCP. Use when the user asks "where do I start", "what order do I read X in", "reading order for X", or wants a guided path through a run or crossover.
---

# Reading order

Give the user an ordered list of issues (or collected editions) they can read start to finish, with a one-line note on each so they know why it's there.

The VerseDB MCP is Pro-only, every tool including search. If a call returns an auth/subscription error (`pro_required` / HTTP 402), tell the user it needs Pro and stop.

## Resolve what they're asking for

Figure out which of these entry points the request maps to, then search:

- **Story arc** → `search-tool` (`type: story_arc`) → `get-tool` (`type: story_arc`) (gives the arc's issues in order).
- **Event / crossover** → `search-tool` (`type: event`) → `get-tool` (`type: event`) (events span many series; expect tie-ins).
- **A run on a Title** ("the Snyder Batman") → `search-tool` (`type: creator`) for the creator + `search-tool` (`type: series`) for the volume, then `get-series-issues-tool` and filter to issues where that creator has the writer role (`get-tool` (`type: issue`) exposes creator roles).
- **A whole series/volume** → `search-tool` (`type: series`) → `get-series-issues-tool`, ordered by issue number.
- **A character's key moments** → `search-tool` (`type: character`) → `get-tool` (`type: character`), then pull the issues; each issue's `key_issue_reasons` from `get-tool` (`type: issue`) flags first appearances, origins, deaths.

If the user names only a Title with several volumes, list the candidate volumes (publisher + year) and ask which one. Don't silently pick.

## Order it

1. Pull the issue set for the resolved entity.
2. Order by in-story chronology: issue number within a volume; for events, lead title first then tie-ins in published order; for multi-volume arcs, follow the arc's own sequence from `get-tool` (`type: story_arc`).
3. Walk every page (25 by default; set `per_page` as high as 100) so long runs aren't truncated.

## Present it

- A numbered list, each line `Series (year) #N` plus a one-line note taken from the issue's `solicitation` when it has one. The MCP carries no plot summaries, so leave the note off rather than writing one from memory. Mark key issues (1st appearance, death, origin) inline.
- Call out optional tie-ins separately from the essential spine.
- If the run is long, offer the collected-edition path (which trades to buy) as an alternative to single issues.
- End by offering to save the order as a list (`list-tool`: `create`, then `add_item`) or add the issues to their pull list / collection.

The server ships a `reading-order-prompt` prompt; prefer it for a straight start-to-finish path.
