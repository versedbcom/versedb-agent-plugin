---
name: creator-spotlight
description: Maps a comic creator's body of work using VerseDB, including their series and issues by role (writer, artist, etc.), signature runs, collaborators, and what to read first. Use when the user asks "what has X written", "X's best work", "bibliography for X", "everything by X", or "where do I start with X as a creator".
---

# Creator spotlight

Take a creator's name and lay out their work by role, then say which run to read first.

The VerseDB MCP is Pro-only, every tool including search. If a call returns an auth/subscription error (`pro_required` / HTTP 402), tell the user it needs Pro and stop.

## Resolve the creator

`search-tool` (`type: creator`), then `get-tool` (`type: creator`). Name clashes are rare, but shared and legacy credits exist, so confirm you have the right person when matches are ambiguous.

## Assemble the body of work

A creator's contribution is **role-scoped**: the same person may write one book and draw another. Group credits by role.

1. Pull the series and issues they're credited on (`get-tool` (`type: creator`), then `search-tool` (`type: issue`) / `get-series-issues-tool` to fill in the full runs). Walk every page (25 by default; set `per_page` as high as 100); a prolific creator's credits run to many pages.
2. Separate by role using the credits on `get-tool` (`type: issue`) (writer vs. penciller/inker/colorist/letterer/cover/editor). Use the `versedb://creator-roles` resource for the canonical labels rather than guessing.
3. Identify **runs**, not scattered single credits. A continuous stretch on one series ("#1–50 of Daredevil") is the unit that matters.
4. Note recurring **collaborators**, like the artist a writer keeps pairing with, and the **publishers** they work with.

## Present it

1. **Who:** one line on primary role(s), era, signature genres and publishers.
2. **Signature runs:** the defining works, each as `Series (years) #range, role, one-line why`. Lead with the canonical starting point.
3. **Full credits by role:** grouped (As writer / As artist / Covers / Editor), runs collapsed into ranges, scannable.
4. **If you liked X, read Y.** Adjacent picks within their catalog.

End by offering to build a reading list of a chosen run (`list-tool`: `create`, then `add_item`), pull the run into the collection, or hand off to `reading-order` for a deep run.
