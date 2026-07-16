---
name: wiki-ingest
description: Ingest user-approved material into the local Markdown-only wiki. Use when adding or updating an evidence-backed wiki article without accessing the internet, APIs, MCP resources, or any source outside the repository and current task input.
---

# Local Wiki Ingestion

Read `AGENTS.md`, `templates/article.md`, and `templates/source.md` before editing. Prefer running under the `wiki-ingest` profile (workspace-write, no network).

1. Accept only material directly supplied in the current task or stored in `inbox/`. Do not retrieve URLs or seek supplementary information.
2. Record provenance first: create or update a source record in `wiki/sources/` (from `templates/source.md`), capturing what the material asserts and its limitations. Use a lowercase, hyphenated filename.
3. Search `wiki/articles/` for an existing article. Update it only when the request authorizes that scope; otherwise create one lowercase, hyphenated Markdown file there from `templates/article.md`.
4. Preserve the required article headings. Write claims only when the recorded source supports them; qualify uncertainty and contradictions rather than resolving them from memory.
5. In `## Sources`, link each claim to its `wiki/sources/` record with a relative link. In `## Related`, add only verified relative links to other published articles.
6. Update `wiki/index.md` last, one article at a time, so the article is discoverable without conflicting with parallel ingestion.

Before handing off, report the changed paths, the provenance recorded in `wiki/sources/`, and every unresolved gap; optionally write this to `reports/ingest-YYYY-MM-DD.md`. Request linting; do not perform an end-user query.
