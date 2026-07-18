---
name: wiki-lint
description: Audit the local Markdown-only wiki for structural, citation, link, and scope problems. Use when checking wiki quality without browsing, external tools, APIs, MCP resources, or model-memory supplementation.
---

# Local Wiki Linting

Read `AGENTS.md`, `DOMAIN.md`, and `templates/README.md`, then inspect only `wiki/**/*.md`. Prefer the `wiki-lint` profile (read-only). Apply the page checks to `wiki/concepts/**/*.md` and `wiki/entities/**/*.md`, and the source checks to `wiki/sources/**/*.md`; treat `wiki/index.md` as navigation, not a page.

Check every published page in `wiki/concepts/` and `wiki/entities/` for:

1. A lowercase, hyphenated filename.
2. A YAML frontmatter block with the keys required for the page type (per `templates/README.md`): concept/entity need `title`, `type`, `tags`, `created`, `updated`, `summary` (entities also `entity_type`). `type` must match the folder (`concepts/` → `concept`, `entities/` → `entity`). Controlled values must be on their allowed lists — `confidence` per the `DOMAIN.md` rubric, and `entity_type` and any `tags`/`link_style` per `DOMAIN.md`; dates must be ISO `YYYY-MM-DD`. **Blocking** when a required key is missing, `type` does not match the folder, or a controlled value is off-list — **except** `tags` that are not on the registry, which stay **advisory** (flag, do not block), and unknown/custom keys, which are **advisory** at most.
3. Exactly one H1 title and exactly one each of `## Summary`, `## Key ideas`, `## Sources`, and `## Related` (both page types share this schema).
4. Non-empty summary, key ideas, and sources sections.
5. Every relative Markdown link resolves to an existing target.
6. Every `## Sources` entry links to a record in `wiki/sources/` or to another published page — not to an outside URL dressed up as local evidence.
7. An entry in `wiki/index.md`.
8. Claims clearly qualified when the linked source record notes that the material is incomplete or contradictory.
9. No leftover template text, generated files, code files, or outside-source claims presented as local evidence.
10. **Advisory (catch-all smell):** flag a page whose `## Key ideas` reads like a list of unrelated topics — a mini table of contents rather than claims about one idea or one named thing. A bundled input belongs in a source record plus one page per item, not a single catch-all page.
11. **Advisory (duplicate/overlap smell):** flag pages that describe the same subject — whether that is a pair or a larger cluster. You cannot embed or hash, so work in two passes the way a human would. First gather candidate pages from cheap signals — near-identical titles or slugs (`attention.md` beside `attention-mechanism.md`), the same named thing living as both a concept and an entity, a shared `entity_type` or heavily overlapping `tags`, or pages leaning on the same source records. Then read only those candidates' `## Summary` and `## Key ideas` closely and decide which they are: the same subject (merge candidates), partly overlapping (better consolidated, or cross-linked under `## Related`), or genuinely distinct (leave them). Group every page that shares a subject into one finding rather than reporting them pair by pair, name them specifically, say which of the three you judged, and recommend the merge or cross-link — never merge or delete a page yourself, and never invent a distinction to keep them.

Check every record in `wiki/sources/` for: a lowercase, hyphenated filename, a stated origin, and specific, citable claims under `## What it asserts`. **Advisory fidelity check** — because linting never reads `inbox/`, judge faithfulness only from what the record itself states: flag a record whose `## Origin` describes a multi-item input (e.g. "a 17-story digest", "a 200-row dataset") but whose `## What it asserts` collapses it into a handful of topic labels instead of one claim per item; and flag claim bullets that name only a broad category with no specific (name, date, quantity, or attribution) when the recorded scale implies specifics exist. Recommend re-ingestion to break the input apart, never invent the missing claims. Flag, too, source records that share the same `## Origin` — a likely double-ingestion — and recommend consolidating them; never merge or delete any yourself.

Report findings only, unless you are explicitly asked to fix them. Label a finding **blocking** when it breaks the corpus contract or a link; otherwise **advisory**. Give each finding a file path, a heading, and a short corrective action; optionally write the summary to `reports/lint-YYYY-MM-DD.md`. Do not rewrite claims or invent evidence.
