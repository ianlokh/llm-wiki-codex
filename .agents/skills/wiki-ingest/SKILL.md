---
name: wiki-ingest
description: Ingest user-approved material into the local Markdown-only wiki. Use when adding or updating an evidence-backed wiki article without accessing the internet, APIs, MCP resources, or any source outside the repository and current task input.
---

# Local Wiki Ingestion

Read `AGENTS.md`, `templates/concept.md`, `templates/entity.md`, and `templates/source.md` before editing. Prefer running under the `wiki-ingest` profile (workspace-write, no network).

1. Accept only material directly supplied in the current task or stored in `inbox/`. Do not retrieve URLs or seek supplementary information.
2. Record provenance first: create or update a source record in `wiki/sources/` (from `templates/source.md`) with a lowercase, hyphenated filename. **Decompose before you summarize** — enumerate the input's distinct substantive claims and record each as its own bullet under `## What it asserts`, preserving the specifics that make it answerable (names, dates, quantities, attribution) and its status (reported/rumored/confirmed/fact). Exclude non-substantive boilerplate (ads, navigation, markup, tracking). State the input's scale and shape in `## Origin` (e.g. "a 17-story news digest", "a 200-row dataset") and its gaps in `## Limitations`. The source record is the faithful evidence layer — never reduce a multi-item input to topic labels.
3. Classify the page. A **concept** explains how an idea, method, or topic works → `wiki/concepts/` from `templates/concept.md`. An **entity** describes one specific named thing — a person, organization, product, tool, model, or place → `wiki/entities/` from `templates/entity.md`. When unsure, prefer `concepts/` for topics and `entities/` for proper nouns that could carry an infobox. When the input bundles many distinct items (a digest, feed, multi-finding report, or dataset), do **not** create one catch-all page: give each item substantial enough to fill the schema its own concept or entity page, and leave lesser items as citable claims in the source record.
4. Search the chosen folder for an existing page. Update it only when the request authorizes that scope; otherwise create one lowercase, hyphenated Markdown file there from the matching template.
5. Preserve the required headings (`## Summary`, `## Key ideas`, `## Sources`, `## Related` — the same for both page types). Write claims only when the recorded source supports them; qualify uncertainty and contradictions rather than resolving them from memory.
6. In `## Sources`, link each claim to its `wiki/sources/` record with a relative link. In `## Related`, add only verified relative links to other published concept or entity pages.
7. Update `wiki/index.md` last, one page at a time, so the page is discoverable without conflicting with parallel ingestion.

Before handing off, verify coverage: confirm that every substantive item you identified in the input is represented as a claim in a source record, and report the count found versus recorded. Then report the changed paths, the provenance recorded in `wiki/sources/`, and every unresolved gap; optionally write this to `reports/ingest-YYYY-MM-DD.md`. Request linting; do not perform an end-user query.
