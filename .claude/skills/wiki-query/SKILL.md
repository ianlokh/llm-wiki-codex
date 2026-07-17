---
name: wiki-query
description: Answer questions exclusively from the completed local Markdown wiki. Use when a response must not browse, call tools outside the repository, use MCP resources, or supplement missing facts with model knowledge or the internet.
---

# Local Wiki Query

Read `AGENTS.md`, then search only published evidence: `wiki/concepts/**`, `wiki/entities/**`, and `wiki/sources/**`. Prefer the `wiki-query` profile (read-only). Treat `wiki/index.md` as navigation, never as evidence.

1. Find the smallest set of passages that directly supports the requested answer.
2. Answer only what those passages establish. Preserve stated uncertainty and disagreement.
3. Attach a local citation to every factual statement as a relative Markdown link whose target is a bare file path with **no `#anchor`** — the link syntax is what makes it clickable in the Markdown-rendering desktop apps this serves (Claude Desktop and the Codex/ChatGPT desktop app), and omitting the anchor keeps the target a real path that resolves to the page rather than a broken fragment. Put the path and heading in the visible text, e.g. `[wiki/concepts/attention.md — Key ideas](wiki/concepts/attention.md)`. Use `wiki/concepts/…`, `wiki/entities/…`, or `wiki/sources/…`; never cite `wiki/index.md`.
4. If no supporting passage exists, answer exactly: `Not found in the local wiki.`
5. If only part of the question is supported, answer that part with citations and state which remainder is not found.

Do not modify files, browse, call external services, recommend external sources, or use unstated knowledge. Do not cite the index as evidence.
