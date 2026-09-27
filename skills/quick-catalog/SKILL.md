---
name: quick-catalog
description: Bulk-adds comics to the user's VerseDB collection from a pasted list, a series plus issue range, a description of a longbox, or a photo of a cover or barcode. It resolves each book and confirms the matches before writing anything. Use when the user says "add these to my collection", "catalog these", "I just bought…", "log this stack", sends a picture of a comic to add, or pastes a list of issues to record.
---

# Quick catalog

Get a stack of comics into the user's collection, matched correctly, asking as few questions as possible. The whole VerseDB MCP is Pro-only. If any call (e.g. `collection-tool` (`operation: add`)) returns an auth/subscription error (`pro_required` / HTTP 402), tell the user it needs Pro and stop.

## Parse the input

Input arrives as one of:
- A **list** of books ("Saga #1–6, Batman 2016 #50, Immortal Hulk #1").
- A **series + range** ("all of The Department of Truth").
- A **loose description** the user types or dictates from a longbox.
- A **photo** of a comic (or several): a cover shot, or a close-up of the barcode.

Normalize each line to `{ title/series, issue number, year?, grade?, condition?, notes? }`. Carry through any grade/condition the user gives; don't invent them.

### Reading a photo

When the input is an image, read what you can off the comic itself before searching:
- **From the cover:** the series title, issue number, publisher trade dress, and any printed cover date or price. That's usually enough to resolve the issue.
- **From the barcode:** read the printed digits and look them up with `lookup-by-barcode-tool` (handles both UPC and ISBN). A single hit resolves the issue directly. Comic UPCs are shared across issues and reprints, and pre-1995 barcodes are unreliable, so the tool often returns several candidates. When it does, match against the cover to pick the right one. If nothing matches, fall back to resolving from the cover.
- A cover photo rarely tells you whether you're holding a **variant** or the base printing. When it's unclear, surface the candidates and let the user confirm, same as any other ambiguous entry.

Tell the user what you read from the image, and ask them to fill in anything the photo can't show (grade/condition, or which variant).

## Match before you write

For each entry:
1. `search-tool` (`type: issue`) (or `search-tool` (`type: series`) → `get-series-issues-tool` for a range) to find candidates.
2. Pick the match only when it's unambiguous (right series volume by **publisher + year**, right issue number). Watch the data-model traps:
   - The same Title has many **volumes**, so "Batman #50" is meaningless without the year/volume.
   - **Variants** are distinct from the base issue, so match the printing the user means; ask if unclear.
   - `#0`, `#-1`, decimal (`#1.1`), and annuals are real, separate issues.
3. **Collect ambiguous and not-found entries into a list. Do not guess.** Resolve everything resolvable first, then present the leftovers for the user to disambiguate in one batch.

## Confirm, then write

- Show the resolved set as a table (`Series (year) #N — grade/condition`) and the unresolved set separately. Get a go-ahead before writing.
- `collection-tool` (`operation: add`) per book. Walk the list; **verify after writing** by reading back (`get-my-collection-tool`) that the count increased by the expected amount.
- Skip duplicates already in the collection rather than double-adding; report them.
- Walk every page (25 by default; set `per_page` as high as 100) of `get-my-collection-tool` for the dupe check and the read-back, and of `get-series-issues-tool` for a long range. Page one alone misses books.

## Report

Summarize: added N, skipped M dupes, K still need the user's input. For the leftovers, give the candidate options so the next round takes one reply.

Offer to record grades/conditions you didn't have, or to start a want-list (`list-tool` (`operation: create`)) for anything they don't own yet. If they're hunting a specific cover, include its `variant_id` on `add_item`; leave it off when any printing will do.
