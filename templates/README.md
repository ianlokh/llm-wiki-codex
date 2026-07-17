# Templates

Skeletons for new wiki content. These live outside `wiki/` so they are never treated as evidence.

- `concept.md` — the required shape for a `wiki/concepts/` entry: an idea, method, framework, or topic (one H1 + `## Summary`, `## Key ideas`, `## Sources`, `## Related`).
- `entity.md` — the required shape for a `wiki/entities/` entry: a person, organization, product, tool, model, or place (same headings as `concept.md`; only the guidance differs).
- `source.md` — the shape for a `wiki/sources/` provenance record.

Copy a template into the target directory and fill it in during ingestion; do not edit content in place here.

## Frontmatter schema

Every published page opens with a YAML frontmatter block (the `--- … ---` header above the H1). It is metadata, never evidence — it is **never cited**, and query answers cite the body, not the frontmatter. Frontmatter makes the corpus queryable by Obsidian Dataview/Bases and by plain readers, without requiring any plugin.

This is a two-layer contract: the **key set below is universal** (identical in every clone of this template), while the **allowed values are domain configuration** and live in `DOMAIN.md`.

**Universal core (all page types):** `title`, `type`, `tags`, `created`/`ingested`, `summary`.

**Per type:**

| Type | Required keys | Optional keys |
| --- | --- | --- |
| concept | `title`, `type: concept`, `tags`, `created`, `updated`, `summary` | `confidence`, `aliases`, `related` |
| entity | `title`, `type: entity`, `entity_type`, `tags`, `created`, `updated`, `summary` | `confidence`, `aliases`, `related` |
| source | `title`, `type: source`, `origin_kind`, `source_format`, `tags`, `item_count`, `claim_count`, `ingested`, `ingested_by` | `summary` |

**Domain-configurable values (see `DOMAIN.md`):** the `tags` registry, the `confidence` scale, the `entity_type` vocabulary, the `origin_kind` vocabulary, which optional fields are enabled, and `link_style`.

### Naming and formatting rules (fixed)

- **Keys are `snake_case`, single-word where possible, never contain spaces** — Dataview silently rewrites spaces to hyphens and then only reaches the field via bracket access, so `item_count`, not `item count`.
- **Dates are unquoted ISO 8601** (`2026-07-17`) so Dataview and Obsidian parse them as real dates.
- **`tags` is the one open axis** — a flat list of registry values. Controlled facets (`type`, `confidence`, `entity_type`, `origin_kind`) live in their own keys, never as tags.
- **Relational keys (`related`, and any `up`) hold bare slugs** — the filename stem of the target, e.g. `related: [backpropagation, micrograd]`. This preserves the Markdown-only, portable-across-apps contract. A clone that lives entirely in Obsidian may set `link_style: wikilink` in `DOMAIN.md` and use `"[[Title]]"` instead, trading portability for native graph resolution.
- **Dates are agent-maintained** here (set by `wiki-ingest`), not by an Obsidian plugin, since this template serves multiple runtimes.
- **Canonical key order** is the order shown in each template; keep it stable for clean diffs.

### Extensibility

Unknown or domain-specific keys (e.g. `prep_time` in a cooking clone) are **permitted** — linting treats keys outside this schema as advisory, never blocking. Add only fields you will actually populate and query; a handful of well-maintained fields beats twenty empty ones. Prefer declaring recurring custom fields in `DOMAIN.md` so the vocabulary stays visible.
