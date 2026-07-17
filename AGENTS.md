# Local Wiki Operating Rules

This repository is a closed, Markdown-only knowledge base. Treat `wiki/**/*.md` as the only authority for factual answers.

## Non-negotiable boundaries

- Do not browse the web, call external APIs, use MCP resources, install packages, or consult any information outside this repository while ingesting, linting, or answering a wiki query.
- The primary enforcement of this is the Codex sandbox: run with `sandbox_mode` set and `sandbox_workspace_write.network_access = false` (see `codex/config.sample.toml`) so the network is unreachable. These written rules are the secondary control if the sandbox is ever misconfigured.
- Do not fill gaps with model memory, general knowledge, or plausible inference. If the corpus does not support an answer, say exactly: `Not found in the local wiki.`
- Cite every substantive answer with a relative Markdown link whose target is a bare file path with no `#anchor`, for example: `[wiki/concepts/attention.md — Key ideas](wiki/concepts/attention.md)`. The link syntax makes citations clickable in the Markdown-rendering desktop apps this serves (Claude Desktop and the Codex/ChatGPT desktop app); omitting the anchor keeps the target a real path that resolves to the page.
- Keep the repository Markdown-only. Do not add source code, databases, generated indexes, lockfiles, or binary assets. Configuration, not code, is the only non-Markdown content permitted: TOML/YAML under `codex/`, `.codex/agents/`, and `.agents/skills/**/agents/`, plus the Claude Code equivalents under `.claude/` (`settings.json` and the agent/skill definitions). The single permitted exception is a CI workflow under `.github/workflows/` that only guards repository invariants — specifically the Claude skill mirror, where each `.claude/skills/<role>/SKILL.md` must stay byte-identical to the canonical `.agents/skills/<role>/SKILL.md`. Codex-provisioned system tooling under `skills/.system/` is machine-managed and git-ignored, not project content.
- Treat material supplied directly in the current task as approved input for ingestion, but record its provenance in `wiki/sources/`. Do not fetch a URL named in that material.

## Domain configuration

Domain-specific knobs — the wiki's purpose, the controlled `tags:` vocabulary, the `confidence` rubric, and the entity/concept scope — live in root `DOMAIN.md`, the single surface for retargeting the wiki to another subject area. These operating-rule files hold only the universal, domain-agnostic rules; consult `DOMAIN.md` for the domain specifics, and edit only `DOMAIN.md` to retarget. `DOMAIN.md` is configuration, never evidence, and is never cited. It is greppable by the ingest/lint/query roles in both runtimes, so it drives behavior rather than merely documenting it.

## Corpus contract

The query surface is **evidence-only**. Everything that is *not* citable evidence lives outside `wiki/`.

- `wiki/concepts/` — published pages for ideas, methods, frameworks, or topics. Lowercase, hyphenated filenames. **Citable.**
- `wiki/entities/` — published pages for a specific named thing: a person, organization, product, tool, model, or place. Lowercase, hyphenated. **Citable.**
- `wiki/sources/` — durable source records that back page claims. Lowercase, hyphenated. **Citable.**
- `wiki/index.md` — a Map of Content for navigation only. **Not evidence; never cite it.**
- `inbox/` — approved-but-unpublished input, outside `wiki/`. **Never evidence.**
- `templates/` — concept, entity, and source templates, outside `wiki/`. **Never evidence.**
- `reports/` — ingestion and lint handoff logs, outside `wiki/`. **Never evidence.**

Because inbox, templates, and reports live outside `wiki/`, the evidence rule is simply: **evidence = `wiki/concepts/**`, `wiki/entities/**`, and `wiki/sources/**`.** The only in-wiki exception is `wiki/index.md`, which is navigation, never evidence.

- Classify each new page before writing it: a **concept** explains how an idea or method works; an **entity** describes one specific named thing (a proper noun that could carry an infobox). When genuinely unsure, prefer `concepts/` for topics and `entities/` for named people, organizations, products, tools, models, or places.
- Every published concept or entity page must contain exactly one H1 title plus these H2 headings exactly once: `## Summary`, `## Key ideas`, `## Sources`, and `## Related`. Both page types share this single schema; the folder — not the headings — carries the concept/entity distinction.
- Every `## Sources` entry must link to a record in `wiki/sources/` or to another published page, using relative Markdown links.
- Use relative Markdown links for all relationships between wiki files.
- Make only additive or narrowly corrective edits. Do not overwrite a human's material without an explicit request.
- Treat `wiki/index.md` as the one serialized write-point: update it last, one page at a time, to avoid parallel-ingestion conflicts.

### Fidelity and aggregation

Source records are the **faithful evidence layer**, not a summary of the input. Abstraction happens on the concept/entity pages built on top — never by discarding specifics at the source. This holds for every kind of input: an article, a report, a news digest, a document, or a dataset.

- **Decompose before you summarize.** Break each approved input into its distinct substantive claims *first*, and record each as its own bullet under a source record's `## What it asserts`, preserving the specifics that make it answerable — names, dates, quantities, and attribution — and its status (reported, rumored, confirmed, or established fact). Exclude non-substantive boilerplate: ads, navigation, markup, tracking, decorative captions.
- **No material loss.** Any fact a reader could reasonably query must be recoverable from a source record. Reducing a multi-item input to topic labels ("covers hardware rumors, pricing, and litigation") produces a table of contents, not evidence, and is a defect.
- **Match granularity to the input.** An input of N distinct items yields N claim clusters, not one. Record the input's scale and shape in the source record's `## Origin` (e.g. "a 17-story news digest", "a 200-row dataset") so that coverage is visible to a reader — and to linting, which cannot see the original input.
- **Aggregations never become one catch-all page.** When an input bundles many distinct items (a news digest, a feed, a multi-finding report, a dataset), it becomes one source record capturing *all* items, plus a separate concept or entity page for each item substantial enough to fill the page schema. Lesser items remain citable as claims in the source record — do not manufacture thin stub pages, and do not create a single page whose `## Key ideas` is a list of unrelated topics.

## Agent roles

Each role is a project-scoped Skill in `.agents/skills/`. Run the smallest number that completes the task; when work is parallelizable, run at most these three independent roles. Each skill sets `allow_implicit_invocation: false`, so a role runs only when explicitly invoked.

| Role | Skill | Recommended profile | May write | Must not do |
| --- | --- | --- | --- | --- |
| Ingestion | `wiki-ingest` | `wiki-ingest` (workspace-write, no network) | New/targeted files in `wiki/concepts/`, `wiki/entities/`, and `wiki/sources/` | Browse or answer end-user questions |
| Linting | `wiki-lint` | `wiki-lint` (read-only) | Nothing unless explicitly asked to fix findings | Invent sources or silently rewrite content |
| Query | `wiki-query` | `wiki-query` (read-only) | Nothing | Browse, infer beyond evidence, or modify the wiki |

Do not run two agents against the same page. Complete ingestion before linting that page. The query agent may read only completed pages.

## Delegation (subagents)

These three roles are also defined as Codex subagents in `.codex/agents/` and may be run under a single orchestrating session.

- When a task spans more than one role (e.g. "ingest this note, lint it, then answer a question"), the orchestrator should delegate each role to its subagent — `wiki-ingest`, `wiki-lint`, `wiki-query` — rather than doing the work inline.
- Respect ordering: ingestion must finish before linting the same page, and linting should pass before that page is queried. Only genuinely independent work (different pages) may run in parallel.
- Each subagent inherits the global no-network sandbox. `wiki-lint` and `wiki-query` additionally run read-only and must not write. Never delegate two writing subagents onto the same article.
- Keep delegation one level deep; a subagent must not spawn further subagents.

## Required handoff

- Ingestion reports changed paths, source provenance recorded in `wiki/sources/`, and unresolved gaps. Logs may be written to `reports/`.
- Linting reports findings with paths and headings, separating blocking from advisory findings.
- Query reports citations for every factual claim and explicitly identifies unsupported portions.
