# VerseDB Agent Plugin

Search, catalog, and discover comics through the [VerseDB](https://versedb.com) MCP. It works with both Claude and OpenAI. The plugin bundles the connection along with a few skills and a couple of agents that know how to use the tools.

VerseDB is a comic database covering comics, manga, manhwa, manhua, and bande dessinée. The MCP gives you 20 tools. Two of them do most of the browsing: `search-tool` and `get-tool` take a `type` parameter covering titles, series, issues, creators, characters, publishers, teams, story arcs, universes, events, comic shops, and podcasts. The rest are specialized lookups (trending, upcoming releases, market prices, barcode lookup, key issues, community reviews, series issue lists) and personal tools for your collection, pull list, reading progress, lists, reviews, and profile. Each write tool takes an `operation` parameter saying what to do.

## What you get

The MCP itself connects at `https://versedb.com/mcp/api`. Installing the plugin wires it up, and your client handles the OAuth sign-in the first time you use it.

### Agents

- `comic-concierge` is the general-purpose helper. It understands how Titles, Series, and Issues relate, searches before it fetches, and asks before writing anything to your account.
- `comic-researcher` is read-only and meant for bigger questions that touch a lot of entities, like tracing a character across eras, mapping out a crossover, or surveying a creator's influence. It comes back with a written report and its sources.

### Skills

- `reading-order` works out the chronological order for a character, arc, event, creator run, or series.
- `collection-insights` looks at collection value, gaps in your runs, reading progress, and the key issues you own.
- `discover-comics` pulls recommendations from trending titles, key issues, and reviews.
- `pull-list-planner` handles upcoming releases, catching up, and tidying your pull list.
- `quick-catalog` bulk-adds comics to your collection from a pasted list, a series plus a range, or a photo of a cover or barcode.
- `character-dossier` builds a character profile: first appearance, key issues, affiliations, and where to start.
- `creator-spotlight` covers a creator's work by role, their signature runs, and what to read first.

## Requirements

You'll need a VerseDB [Pro subscription](https://versedb.com/pro). The hosted MCP requires Pro for **every** tool, search and browse included. Without it, calls come back with a `pro_required` / HTTP 402 error.

## Data and privacy

The plugin doesn't read, store, or send any credential from your machine. There are no API keys,
environment variables, or config files involved. The one connection it declares is the VerseDB MCP
at `https://versedb.com/mcp/api`, and you sign in to it through your client's own OAuth flow. The
client keeps that token and uses it only for versedb.com.

Tool calls go to VerseDB and nowhere else. What VerseDB does with your account data is covered by
its [privacy policy](https://versedb.com/policy/privacy).

## Install

The plugin lives in a marketplace on GitHub (`versedbcom/versedb-agent-plugin`). You add the
marketplace once, then install VerseDB from it. The steps differ a little depending on where you
run it.

### Claude website (claude.ai) or the Claude desktop app

1. Open **Customize** in the left sidebar.
2. Next to **Personal plugins**, click the **+** button.
3. Choose **Create plugin**, then **Add marketplace**.
4. Pick **Add from a repository**. In the URL field, paste the GitHub repo:
   `versedbcom/versedb-agent-plugin`
5. Click **Sync**. (Claude warns that marketplace plugins aren't built or vetted by Anthropic.
   That's expected.)
6. Once it syncs, find **VerseDB** in the plugin list and click **Install**.

Plugins are on the paid plans (Pro, Max, Team, Enterprise). The first time you use a VerseDB tool,
Claude walks you through signing in to your account.

### Claude Code (command line)

```text
/plugin marketplace add versedbcom/versedb-agent-plugin
/plugin install versedb@versedb
```

To test a local checkout instead:

```text
claude --plugin-dir /path/to/versedb-agent-plugin
```

### OpenAI

Find **VerseDB** in the OpenAI plugin directory and install it. You'll be asked to sign in to
your VerseDB account the first time a tool runs. On OpenAI the two agents ship as skills of the
same name (`comic-concierge` and `comic-research`), since agents aren't supported there.

## Usage

Just ask for what you want in plain language: "what order do I read Saga in", "what's my collection worth", "what's coming out for my pull list", "recommend something like Immortal Hulk". The concierge and the skills figure out which tools to call.
