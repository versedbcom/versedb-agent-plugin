---
name: character-dossier
description: Builds a profile of a comic character using VerseDB, covering first appearance and key issues, teams and affiliations, the story arcs and events they anchor, and where to start reading. Use when the user asks "tell me about X", "who is X", "X's first appearance", "X's key issues", or "where do I start with X".
---

# Character dossier

Give a reader or collector a sourced profile they can use: who the character is, which issues matter, and where to start reading.

The VerseDB MCP is Pro-only, every tool including search. If a call returns an auth/subscription error (`pro_required` / HTTP 402), tell the user it needs Pro and stop.

## Resolve the character

`search-tool` (`type: character`) first. Common names collide across publishers and legacies (think of all the Robins, or the various Captain Marvels). When several strong matches come back, show them with publisher/universe and let the user pick. Then `get-tool` (`type: character`) for the core record.

## Build the dossier

Gather each piece from its own source. Walk every page (25 by default; set `per_page` as high as 100) on every list; a character with decades of history won't fit on one page.

- **First appearance & key issues:** surface the debut and other significant books, labeled with their significance from each issue's `key_issue_reasons` in `get-tool` (`type: issue`): 1st appearance, origin, death, first cover, and so on. `get-key-issue-reasons-tool` only searches the reason names ("First appearance of Punisher"); it doesn't say which issue carries one.
- **Affiliations:** teams they belong to (`search-tool` (`type: team`) / `get-tool` (`type: team`)) and the universe they live in (`get-tool` (`type: universe`)).
- **Defining stories:** the story arcs and events they anchor (`search-tool` (`type: story_arc`), `search-tool` (`type: event`), `get-tool` (`type: event`)).
- **Notable runs:** the series where they're central (`search-tool` (`type: series`)), plus the creators most associated with them.

## Present it

Structure it so it scans:
1. **Who:** one paragraph on publisher, universe, alter ego, and debut year.
2. **Key issues:** bulleted, each with its significance and issue id, ordered by importance (1st appearance first). Flag which are expensive, cross-referencing `get-market-prices-tool` only if the user cares about value.
3. **Affiliations:** teams and universe.
4. **Defining stories:** arcs and events, in suggested reading order.
5. **Where to start:** one series plus issue, or a collected edition. Hand off to the `reading-order` skill for anything complex.

End by offering to save the key issues as a want-list, add them to the user's collection, or build a reading list.
