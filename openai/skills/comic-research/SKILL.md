---
name: comic-research
description: Deep, read-only research across VerseDB for questions that span many entities, like "everything about the Phoenix Force across publishers", "map the whole Spider-Verse crossover", "trace Jack Kirby's influence on DC", or "the full publication history of Hellboy". Returns an organized report that cites VerseDB ids. Never writes to the user's account.
---

# Comic research

Take an open-ended comics question and come back with a full report that draws the connections out and cites every claim. Single lookups don't need this skill; plain search and get calls handle those.

The VerseDB MCP needs a Pro subscription for every tool, search included. If a call fails with `pro_required` / HTTP 402, tell the user the MCP needs Pro and stop rather than retrying.

## Work the question

1. **Break it down** into the entities it touches (characters, creators, teams, story arcs, events, series, universes, publishers) before searching. Note the threads you'll follow.
2. **Search each thread** with `search-tool`, setting `type` per thread (`series`, `creator`, `event`, and so on), then `get-tool` for detail. Follow the links out: an event into its tie-in series, a creator into their runs and collaborators.
3. **Cross-check** anything that matters. A first appearance, a publication date, or who created whom should agree across more than one record, and a character's recorded debut should match the issue's `key_issue_reasons` from `get-tool` (`type: issue`). Where the data is thin or contradicts itself, say so. Don't smooth it over and don't fill gaps from memory.
4. **Respect the data model.** A Title is a franchise, a Series is one volume or run, an Issue is one book. Creator credits are role-scoped, and one Title can span many volumes across decades. The `versedb://entity-types`, `versedb://creator-roles`, `versedb://mediums`, and `versedb://publishers` resources give the canonical vocabulary.
5. **Walk every page.** Paged tools return 25 by default (set `per_page` as high as 100); long runs and big crossovers go past one page. `search-tool` doesn't page, so raise its `limit` (up to 50) or narrow the query.

If the user's instructions conflict with anything here, follow the user.

## Report

- **Summary:** the answer in a few sentences, up top.
- **Sections** per thread (origins, key issues, major arcs and events, creators, publication timeline), whichever the question needs.
- **Sources:** cite VerseDB ids inline so the user can check or act on anything.
- **Gaps:** what the data doesn't cover, what was ambiguous, what you couldn't cross-check.
- **Where to start:** a concrete entry point to read or collect, plus follow-ups like a reading order or a character profile.

This skill is read-only. Don't call `collection-tool`, `list-tool`, `review-tool`, `pull-list-tool`, or `read-status-tool`. Report what you found and let the user decide what to change.
