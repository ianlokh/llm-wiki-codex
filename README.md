# Closed-World Wiki — a reusable, Markdown-only knowledge base template

A local-first, **Markdown-only** knowledge base you clone and point at any subject area. It is intentionally **closed-world**: answers come only from the pages in `wiki/`, and a missing answer is reported as missing — never supplemented from the internet or model memory.

It runs identically under **two AI runtimes** — Codex (`AGENTS.md`) and Claude Code (`CLAUDE.md`) — and the pages are plain YAML-frontmatter Markdown, so the same vault opens cleanly in **Obsidian**, Claude Desktop, or the Codex/ChatGPT desktop app.

> **New here, or not a developer?** Start with **[GETTING-STARTED.md](GETTING-STARTED.md)** — a plain-language walkthrough from download to your first cited answer, including a worked example. Common questions are answered in **[FAQ.md](FAQ.md)**.

Two files carry everything you customize:

- **`DOMAIN.md`** — the single surface for retargeting the wiki to your subject (purpose, tag vocabulary, confidence rubric, entity/concept scope, frontmatter values).
- **`templates/README.md`** — the universal frontmatter schema (fixed across clones).

Everything else — the evidence-only corpus contract, the folder split, the tool-gated roles — stays universal.

---

## Customizing for your domain

**To retarget the wiki, edit only [`DOMAIN.md`](DOMAIN.md).** It holds every domain-specific knob:

| Knob | What it controls |
| --- | --- |
| **Purpose** | One paragraph describing the subject area. |
| **Tag taxonomy** | The controlled `tags:` registry — a governed-growth list, reuse-first. |
| **Confidence rubric** | What `high` / `medium` / `low` mean for your material. |
| **Entity scope** | Allowed `entity_type` values (product, organization, person, …). |
| **Concept scope** | What counts as a concept vs. an entity. |
| **Page metadata** | `origin_kind` vocabulary, which optional frontmatter fields are enabled, and `link_style`. |

The operating-rule files (`AGENTS.md`, `CLAUDE.md`) and the templates never mention your subject — they defer to `DOMAIN.md` for every specific. Clone, rewrite `DOMAIN.md`, and the roles behave in the new domain with no other edits.

### Example domains

The shipped `DOMAIN.md` targets **contemporary technology / applied AI**. The same machinery works for, e.g.:

- **Cooking** — concepts = techniques (emulsification), entities = ingredients/tools/dishes, sources = recipes/books; add a custom `prep_time` field.
- **Competitive analysis** — concepts = strategies, entities = companies/products, sources = filings/press; lean on `confidence` for rumor vs. confirmed.
- **Course notes** — concepts = topics, entities = people/works, sources = lectures/readings; use `related` to build a study graph.
- **Security research** — concepts = attack/defense methods, entities = threats/tools/actors, sources = advisories/reports.

---

## The corpus contract (universal)

The query surface is **evidence-only**. Everything citable lives in exactly three folders:

- `wiki/concepts/` — ideas, methods, frameworks, topics. **Citable.**
- `wiki/entities/` — one specific named thing (person, org, product, tool, model, place). **Citable.**
- `wiki/sources/` — durable provenance records that back page claims. **Citable.**

`wiki/index.md` (Map of Content) and anything outside `wiki/` (`inbox/`, `templates/`, `reports/`) are **never evidence**. Every published page opens with a YAML frontmatter block and the fixed heading schema (`## Summary`, `## Key ideas`, `## Sources`, `## Related`).

---

## Frontmatter & analytics

Every page carries queryable YAML frontmatter — the substrate for dashboards and analytics.

- The **key set is universal** (defined in [`templates/README.md`](templates/README.md)); the **allowed values are your domain** (in `DOMAIN.md`).
- It is **plugin-agnostic plain YAML**: readable everywhere, and directly queryable by Obsidian Dataview/Bases where present. Frontmatter is metadata — never cited; answers come from the body.
- Relational links (`related`) use **portable bare slugs** by default (`link_style: slug`); Obsidian-only vaults can switch to `wikilink`.

### Obsidian plugin setup (optional)

1. Open the `wiki/` folder as an Obsidian vault.
2. Install the **Dataview** community plugin (add **Charts** for visualizations).
3. Drop a query into `wiki/index.md` (never cited, so it's the natural dashboard):

   ````markdown
   ```dataview
   TABLE confidence, updated, summary
   FROM "concepts" OR "entities"
   WHERE confidence = "low"
   SORT updated DESC
   ```
   ````

   Group sources by shape, or sum coverage:

   ````markdown
   ```dataview
   TABLE origin_kind, item_count, claim_count FROM "sources" SORT item_count DESC
   ```
   ````

No plugin is required for the wiki to function — Dataview only adds live dashboards on top of the same files.

---

## Running it

Two runtimes read the **same** corpus and roles. Three tool-gated roles do the work: **ingest** (writes the corpus; no network), **lint** (read-only audit), **query** (read-only, wiki-only answers).

### Codex

Codex loads config from `$CODEX_HOME` (default `~/.codex/`), not from the repo, so `codex/` holds templates you apply once:

1. **Enforce closed-world + register skills.** Merge `codex/config.sample.toml` into `~/.codex/config.toml` (sets `sandbox_workspace_write.network_access = false` and enables the three skills).
2. **Install per-role profiles.** Copy each file in `codex/profiles/` into `$CODEX_HOME`, e.g. `cp codex/profiles/wiki-query.config.toml ~/.codex/wiki-query.config.toml`.

`AGENTS.md` and `.agents/skills/` are auto-loaded. Then:

- Ingest: `codex --profile wiki-ingest` → invoke `wiki-ingest`.
- Audit: `codex --profile wiki-lint` → invoke `wiki-lint`.
- Query: `codex --profile wiki-query` → invoke `wiki-query`.

The query/lint profiles use a `read-only` sandbox; ingest allows writes but keeps the network unreachable.

### Claude Code

Open the folder in Claude Code. It reads `CLAUDE.md`, and the three subagents in `.claude/agents/` are **tool-gated** (ingest gets write tools, lint/query are read-only; none get web tools). Invoke a role with the Task tool, or run its skill from `.claude/skills/`.

**Skill mirror invariant:** the canonical skills live in `.agents/skills/<role>/SKILL.md`; the `.claude/skills/<role>/SKILL.md` copies must stay **byte-identical**. Edit the canonical, then `cp` it over the mirror. `.github/workflows/skill-mirror.yml` enforces this on every push/PR.

---

## Corpus flow

Place approved Markdown in `inbox/` → **`wiki-ingest`** converts it into a concept or entity page plus a `wiki/sources/` provenance record (each with frontmatter) → **`wiki-lint`** audits structure, frontmatter, links, and scope → **`wiki-query`** answers from the completed pages. Ingest before lint; lint before query; update `wiki/index.md` last.

---

## Layout

```
GETTING-STARTED.md     Plain-language quickstart for non-technical users
FAQ.md                 Common questions and answers
LICENSE                MIT license
AGENTS.md              Universal operating contract — Codex
CLAUDE.md              Universal operating contract — Claude Code (mirror of AGENTS.md)
DOMAIN.md              ← THE customization surface: retarget the wiki here
wiki/                  EVIDENCE ONLY — the entire query surface
  index.md             Map of Content / dashboard host (navigation, never cited)
  concepts/            Ideas / methods / topics (citable) — generated, gitignored
  entities/            People / orgs / products / tools (citable) — generated, gitignored
  sources/             Provenance records backing page claims (citable) — generated, gitignored
inbox/                 Approved-but-unpublished input (never evidence); ships with a worked-example article
examples/              Expected output of the worked example (never evidence)
templates/             Page templates + frontmatter schema (README.md) (never evidence)
reports/               Ingest/lint handoff logs (never evidence)
.agents/skills/        Canonical role skills (Codex auto-discovers)
.claude/               Claude Code skills (byte-identical mirrors) + tool-gated agents
.codex/ , codex/       Codex agents + TOML profile templates
.github/workflows/     CI that guards the skill-mirror invariant
```

> **Note:** generated pages under `wiki/{concepts,entities,sources}/` are gitignored — the template ships the scaffolding, and each clone grows its own local corpus.
