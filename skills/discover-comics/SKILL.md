---
name: discover-comics
description: Recommends comics to read or buy from VerseDB, drawing on trending titles, key issues, and well-reviewed books, tuned to a genre, character, creator, or the user's tastes, and can save the picks to a list. Use when the user asks "what should I read", "what's popular", "recommend something like X", "what's hot right now", or "find me good X comics".
---

# Discover comics

Recommend books the user will actually finish, and back every pick with a reason.

The VerseDB MCP is Pro-only, every tool including search. If a call returns an auth/subscription error (`pro_required` / HTTP 402), tell the user it needs Pro and stop.

## Gather signal

Pull from several sources and balance them; don't lean on one. Start with `get-trending-tool`: `top_rated`, `most_reviewed`, or `recent_popular` for series that started in the last two years. It ranks on member ratings, not sales. Once you have a shortlist, check each series with `get-community-reviews-tool` (`series_id`) for the most-liked member reviews, and quote them as readers' opinions, not fact. For historically significant books, read `key_issue_reasons` on candidate issues with `get-tool` (`type: issue`). When the user anchors on a creator, character, or publisher ("more like Saga", "anything by Tom King"), go straight to `search-tool` with the matching `type` (`creator`, `character`, `publisher`, `series`).

Walk every page of any list you pull before ranking (25 by default; set `per_page` as high as 100); a truncated set skews the picks. `search-tool` doesn't page, so raise its `limit` (up to 50) or narrow the query.

## Tune to the user

- If they name a reference point ("like X"), resolve X first (`search-tool` (`type: series`) / `get-tool` (`type: series`)) and match on creators, genre, publisher, and tone.
- If they have a collection or lists, read `get-my-collection-tool` / `get-my-lists-tool` to avoid recommending what they already own and to infer taste.
- Respect stated constraints: medium (comic/manga/manhwa/etc.), all-ages vs. mature, ongoing vs. complete, single issues vs. collected editions.

## Present it

A short ranked list, 5–8 picks rather than a dump. Write each as `Series (year), one-line why (the signal behind it)`. Group them loosely: a few safe bets, then a couple of wilder swings. Say where to start, tying in the `reading-order` skill for anything with a complex entry point. If you quote a value, state the grade; prices are grade-dependent.

End by offering to save picks to a list (`list-tool`: `create`, then `add_item`) or add to their pull list or collection. Confirm before writing.
