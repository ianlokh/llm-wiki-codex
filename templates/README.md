# Templates

Starting shapes for new wiki content. These live outside `wiki/` so they are never treated as evidence.

- `concept.md` — the required shape for a `wiki/concepts/` entry: an idea, method, framework, or topic (one H1 + `## Summary`, `## Key ideas`, `## Sources`, `## Related`).
- `entity.md` — the required shape for a `wiki/entities/` entry: a person, organization, product, tool, model, or place (same headings as `concept.md`; only the guidance differs).
- `source.md` — the shape for a `wiki/sources/` provenance record.

Copy a template into the target folder and fill it in during ingestion; do not fill in content here in the template itself.

## Frontmatter schema

Every published page opens with a YAML frontmatter block (the `--- … ---` header above the H1). It is information about the page, never evidence — it is **never cited**, and query answers cite the body, not the frontmatter. Frontmatter is what lets tools like Obsidian Dataview/Bases — and plain readers — search the wiki, without requiring any plugin.

This has two layers: the **set of keys below is the same in every clone** of this template, while the **allowed values depend on your subject** and live in `DOMAIN.md`.

**Universal core (all page types):** `title`, `type`, `tags`, `created`/`ingested`, `summary`.

**Per type:**

| Type | Required keys | Optional keys |
| --- | --- | --- |
| concept | `title`, `type: concept`, `tags`, `created`, `updated`, `summary` | `confidence`, `aliases`, `related` |
| entity | `title`, `type: entity`, `entity_type`, `tags`, `created`, `updated`, `summary` | `confidence`, `aliases`, `related` |
| source | `title`, `type: source`, `origin_kind`, `source_format`, `tags`, `item_count`, `claim_count`, `ingested`, `ingested_by` | `summary` |

**Values you set per subject (see `DOMAIN.md`):** the `tags` registry, the `confidence` scale, the `entity_type` vocabulary, the `origin_kind` vocabulary, which optional fields are enabled, and `link_style`.

### Naming and formatting rules (fixed)

- **Keys are `snake_case`, one word where possible, and never contain spaces** — Dataview quietly turns spaces into hyphens and can then only reach the field through bracket access, so write `item_count`, not `item count`.
- **Dates are unquoted ISO 8601** (`2026-07-17`) so Dataview and Obsidian read them as real dates.
- **`tags` is the one open-ended field** — a flat list of registry values. The fixed categories (`type`, `confidence`, `entity_type`, `origin_kind`) each get their own key, never a tag.
- **Link keys (`related`, and any `up`) hold bare slugs** — the target's file name without `.md`, e.g. `related: [backpropagation, micrograd]`. This keeps the wiki Markdown-only and portable across apps. A clone that lives entirely in Obsidian may set `link_style: wikilink` in `DOMAIN.md` and use `"[[Title]]"` instead, giving up portability for Obsidian's built-in graph links.
- **Dates are set by the agent** here (by `wiki-ingest`), not by an Obsidian plugin, because this template runs in more than one app.
- **Keep the key order** shown in each template; a stable order keeps diffs clean.

### Extensibility

Unknown or subject-specific keys (e.g. `prep_time` in a cooking clone) are **allowed** — linting treats keys outside this schema as advisory, never blocking. Add only fields you will actually fill in and query; a few well-kept fields beat twenty empty ones. List recurring custom fields in `DOMAIN.md` so the vocabulary stays visible.
