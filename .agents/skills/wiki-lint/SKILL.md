---
name: wiki-lint
description: Audit the local Markdown-only wiki for structural, citation, link, and scope problems. Use when checking wiki quality without browsing, external tools, APIs, MCP resources, or model-memory supplementation.
---

# Local Wiki Linting

Read `AGENTS.md`, then inspect only `wiki/**/*.md`. Prefer the `wiki-lint` profile (read-only). Apply page checks to `wiki/concepts/**/*.md` and `wiki/entities/**/*.md`, and source checks to `wiki/sources/**/*.md`; treat `wiki/index.md` as navigation, not a page.

Check every published page in `wiki/concepts/` and `wiki/entities/` for:

1. A lowercase, hyphenated filename.
2. Exactly one H1 title and one each of `## Summary`, `## Key ideas`, `## Sources`, and `## Related` (both page types share this schema).
3. Non-empty summary, key ideas, and sources sections.
4. Every relative Markdown link resolves to an existing target.
5. Every `## Sources` entry links to a record in `wiki/sources/` or to another published page — not to an external URL presented as local evidence.
6. An entry in `wiki/index.md`.
7. Claims visibly qualified when the linked source record notes incompleteness or contradiction.
8. No template content, generated artifacts, code files, or external-source claims presented as local evidence.
9. **Advisory (catch-all smell):** flag a page whose `## Key ideas` reads as a list of unrelated topics — a mini-index rather than claims about one idea or one named thing. Aggregations belong in a source record plus per-item pages, not a single catch-all page.

Check every record in `wiki/sources/` for: a lowercase, hyphenated filename, a stated origin, and specific, citable claims under `## What it asserts`. **Advisory fidelity check** — because linting never reads `inbox/`, judge fidelity intrinsically from what the record itself states: flag a record whose `## Origin` describes a multi-item input (e.g. "a 17-story digest", "a 200-row dataset") but whose `## What it asserts` collapses it into a handful of topic labels instead of per-item claims; and flag claim bullets that name only a broad category with no specific (name, date, quantity, or attribution) when the recorded scale implies specifics exist. Recommend re-ingestion to decompose the input, never invent the missing claims.

Return findings only unless explicitly asked to fix them. Label a finding **blocking** when it breaks the corpus contract or a link; otherwise **advisory**. Give each finding a file path, heading, and concise corrective action; optionally write the summary to `reports/lint-YYYY-MM-DD.md`. Do not rewrite claims or invent evidence.
