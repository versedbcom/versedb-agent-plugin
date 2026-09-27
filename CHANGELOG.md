# Changelog

## 0.1.12

- Update

## 0.1.11

- Update

## 0.1.10

- Update

## 0.1.9

- Repository renamed to versedb-agent-plugin; display name is now VerseDB: Comic Book Tracker.

## 0.1.8

- The concierge can rate a whole series (stars only) as well as review single issues.
- Key issues now come from each issue's own `key_issue_reasons`. Several skills were asking `get-key-issue-reasons-tool` for them, which only searches the reason names.
- Pull-list planning asks the server for just the series on your pull list and walks every page, so a busy release week no longer drops books.
- Collection insights uses the collection summary for totals and says where a value came from.
- Reading orders no longer write plot notes the data doesn't carry; the note comes from the solicitation or is left off.
- Pagination guidance matches the server: 25 per page by default, up to 100.
- README lists all 20 tools, including the profile tool.

## 0.1.7

- Update

## 0.1.6

- Update

## 0.1.5

- Update

## 0.1.4

- Update

## 0.1.3

- Fixed broken YAML frontmatter in the collection-insights and pull-list-planner skills. A stray colon in the `description` line kept those two skills from loading their metadata, so Claude couldn't pick them up on its own. Both work now.
- Moved `.mcp.json` to the standard `{ "mcpServers": { ... } }` shape and dropped the redundant `mcpServers` path from the manifest — the file is auto-discovered at the plugin root.
- Added a `$schema` reference to `plugin.json` for editor validation.
- Rounded out the keyword list (manga, pull-list, market-prices).

## 0.1.2

- Earlier release.
