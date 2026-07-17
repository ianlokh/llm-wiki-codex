# Local Wiki Operating Rules (Claude Code)

This repository is a closed, Markdown-only knowledge base. Treat `wiki/**/*.md` as the only authority for factual answers. This file mirrors `AGENTS.md`; the two must stay in sync so the wiki behaves identically under Codex and Claude Code.

## Non-negotiable boundaries

- Do not browse the web, call external services, or consult any information outside this repository while ingesting, linting, or querying.
- Enforcement here is by **tool gating**: the role subagents in `.claude/agents/` are granted no web tools, and the lint/query roles are granted no write tools. Do not work around this by using the main session's broader tools to fetch or to edit under a read-only role.
- Do not fill gaps with model memory, general knowledge, or inference. If the corpus does not support an answer, say exactly: `Not found in the local wiki.`
- Cite every substantive answer with a relative Markdown link whose target is a bare file path with no `#anchor`, e.g. `[wiki/concepts/attention.md — Key ideas](wiki/concepts/attention.md)`. The link syntax makes citations clickable in the Markdown-rendering desktop apps this serves (Claude Desktop and the Codex/ChatGPT desktop app); omitting the anchor keeps the target a real path that resolves to the page.
- Keep the repository Markdown-only. TOML/YAML under `codex/`, `.codex/agents/`, and `.agents/skills/**/agents/`, the Claude Code config under `.claude/` (`settings.json` and the agent/skill definitions), and a CI workflow under `.github/workflows/` that only guards repository invariants (the skill mirror described under Skills), are configuration, not code. Codex-provisioned system tooling under `skills/.system/` is machine-managed and git-ignored, not project content.

## Corpus contract

The query surface is **evidence-only**; everything that is not citable evidence lives outside `wiki/`.

- `wiki/concepts/` — published pages for ideas, methods, frameworks, or topics (lowercase, hyphenated). **Citable.**
- `wiki/entities/` — published pages for a specific named thing: a person, organization, product, tool, model, or place (lowercase, hyphenated). **Citable.**
- `wiki/sources/` — durable source records backing page claims. **Citable.**
- `wiki/index.md` — Map of Content, navigation only. **Never cite it.**
- `inbox/`, `templates/`, `reports/` — outside `wiki/`. **Never evidence.**

So the rule is simply: **evidence = `wiki/concepts/**`, `wiki/entities/**`, and `wiki/sources/**`.** `wiki/index.md` is the one in-wiki file that is never evidence.

- Classify each new page: a **concept** explains how an idea or method works; an **entity** describes one named thing. When unsure, prefer `concepts/` for topics and `entities/` for proper nouns that could carry an infobox.
- Every published concept or entity page has exactly one H1 plus one each of `## Summary`, `## Key ideas`, `## Sources`, `## Related`. Both page types share this schema; the folder carries the concept/entity distinction.
- Every `## Sources` entry links to a record in `wiki/sources/` or another published page, via relative Markdown links.
- Make only additive or narrowly corrective edits. Update `wiki/index.md` last, one page at a time.

### Fidelity and aggregation

Source records are the **faithful evidence layer**, not a summary of the input. Abstraction happens on the concept/entity pages built on top — never by discarding specifics at the source. This holds for every kind of input: an article, a report, a news digest, a document, or a dataset.

- **Decompose before you summarize.** Break each approved input into its distinct substantive claims *first*, and record each as its own bullet under a source record's `## What it asserts`, preserving the specifics that make it answerable — names, dates, quantities, and attribution — and its status (reported, rumored, confirmed, or established fact). Exclude non-substantive boilerplate: ads, navigation, markup, tracking, decorative captions.
- **No material loss.** Any fact a reader could reasonably query must be recoverable from a source record. Reducing a multi-item input to topic labels ("covers hardware rumors, pricing, and litigation") produces a table of contents, not evidence, and is a defect.
- **Match granularity to the input.** An input of N distinct items yields N claim clusters, not one. Record the input's scale and shape in the source record's `## Origin` (e.g. "a 17-story news digest", "a 200-row dataset") so that coverage is visible to a reader — and to linting, which cannot see the original input.
- **Aggregations never become one catch-all page.** When an input bundles many distinct items (a news digest, a feed, a multi-finding report, a dataset), it becomes one source record capturing *all* items, plus a separate concept or entity page for each item substantial enough to fill the page schema. Lesser items remain citable as claims in the source record — do not manufacture thin stub pages, and do not create a single page whose `## Key ideas` is a list of unrelated topics.

## Roles and delegation (subagents)

Three subagents in `.claude/agents/` mirror the Codex roles. Invoke them with the Task tool; do the work through them rather than inline when a task spans roles.

| Role | Subagent | Tools granted | Enforces |
| --- | --- | --- | --- |
| Ingestion | `wiki-ingest` | Read, Write, Edit, Glob, Grep | Can write the corpus; no web |
| Linting | `wiki-lint` | Read, Glob, Grep | Read-only; reports findings |
| Query | `wiki-query` | Read, Glob, Grep | Read-only; wiki-only answers |

Ordering: ingestion must finish before linting the same page, and linting should pass before that page is queried. Only independent pages may be processed in parallel. Never point two writing subagents at the same page. Subagents must not spawn further subagents.

## Skills

The canonical skills live in `.agents/skills/{wiki-ingest,wiki-lint,wiki-query}/SKILL.md`, which Codex auto-discovers by location. Claude Code only reads skills under `.claude/skills/`, and git symlinks are not portable to Windows checkouts, so `.claude/skills/{wiki-ingest,wiki-lint,wiki-query}/SKILL.md` are **committed real-file copies** (mirrors) of the canonicals — not symlinks.

**Invariant:** each `.claude/skills/<role>/SKILL.md` must stay byte-identical to `.agents/skills/<role>/SKILL.md`. Edit the canonical under `.agents/skills/`, then copy it over the mirror:
- macOS/Linux: `cp .agents/skills/<role>/SKILL.md .claude/skills/<role>/SKILL.md`
- Windows PowerShell: `Copy-Item .agents/skills/<role>/SKILL.md .claude/skills/<role>/SKILL.md -Force`

`.github/workflows/skill-mirror.yml` enforces this on every push and pull request: it fails, with the exact copy command, if a mirror drifts from its canonical or is replaced by a symlink. The Claude subagents in `.claude/agents/` load their role skill by name via the `skills:` frontmatter field (which preloads the mirror from `.claude/skills/`), so no in-agent path reference is needed.
