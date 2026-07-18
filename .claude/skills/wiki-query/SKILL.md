---
name: wiki-query
description: Answer questions exclusively from the completed local Markdown wiki. Use when a response must not browse, call tools outside the repository, use MCP resources, or supplement missing facts with model knowledge or the internet.
---

# Local Wiki Query

Read `AGENTS.md`, then search only the published evidence: `wiki/concepts/**`, `wiki/entities/**`, and `wiki/sources/**`. Prefer the `wiki-query` profile (read-only). Treat `wiki/index.md` as navigation, never as evidence.

1. Find the fewest passages that directly support the answer being asked for.
2. Answer only what those passages actually establish. Keep any uncertainty or disagreement they state.
3. Give every factual statement a local citation, written as a relative Markdown link whose target is a plain file path with **no `#anchor`** — writing it as a link is what makes it clickable in the Markdown-rendering desktop apps this serves (Claude Desktop and the Codex/ChatGPT desktop app), and leaving off the anchor keeps the target a real path that resolves to the page rather than a broken fragment. Put the path and heading in the visible text, e.g. `[wiki/concepts/attention.md — Key ideas](wiki/concepts/attention.md)`. Use `wiki/concepts/…`, `wiki/entities/…`, or `wiki/sources/…`; never cite `wiki/index.md`.
4. If no supporting passage exists, answer exactly: `Not found in the local wiki.`
5. If only part of the question is supported, answer that part with citations and say which remainder is not found.

Do not change files, browse, call external services, recommend outside sources, or use knowledge that is not in the wiki. Do not cite the index as evidence.
