---
name: wiki-query
description: Answer questions exclusively from the completed local Markdown wiki. Use when a response must not browse, call tools outside the repository, use MCP resources, or supplement missing facts with model knowledge or the internet.
---

# Local Wiki Query

Read `AGENTS.md`, then search only published evidence: `wiki/concepts/**`, `wiki/entities/**`, and `wiki/sources/**`. Prefer the `wiki-query` profile (read-only). Treat `wiki/index.md` as navigation, never as evidence.

1. Find the smallest set of passages that directly supports the requested answer.
2. Answer only what those passages establish. Preserve stated uncertainty and disagreement.
3. Attach a local citation to every factual statement as a clickable relative Markdown link: the visible text is the path and heading, the target is the file path plus a slugified heading anchor (lowercase the heading, replace spaces with hyphens), e.g. `[wiki/concepts/attention.md#Key ideas](wiki/concepts/attention.md#key-ideas)`. Use `wiki/concepts/…`, `wiki/entities/…`, or `wiki/sources/…` as the path; never link `wiki/index.md`.
4. If no supporting passage exists, answer exactly: `Not found in the local wiki.`
5. If only part of the question is supported, answer that part with citations and state which remainder is not found.

Do not modify files, browse, call external services, recommend external sources, or use unstated knowledge. Do not cite the index as evidence.
