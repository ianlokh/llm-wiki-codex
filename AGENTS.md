# Local Wiki Operating Rules

This repository is a closed, Markdown-only knowledge base. Treat `wiki/**/*.md` as the only authority for factual answers.

## Non-negotiable boundaries

- Do not browse the web, call external APIs, use MCP resources, install packages, or consult any information outside this repository while ingesting, linting, or answering a wiki query.
- The primary enforcement of this is the Codex sandbox: run with `sandbox_mode` set and `sandbox_workspace_write.network_access = false` (see `codex/config.sample.toml`) so the network is unreachable. These written rules are the secondary control if the sandbox is ever misconfigured.
- Do not fill gaps with model memory, general knowledge, or plausible inference. If the corpus does not support an answer, say exactly: `Not found in the local wiki.`
- Cite every substantive answer with a local path and heading, for example: `wiki/articles/attention.md#Key ideas`.
- Keep the repository Markdown-only. Do not add source code, databases, generated indexes, lockfiles, or binary assets. TOML/YAML under `codex/` and `.agents/skills/**/agents/` are configuration, not code, and are the only non-Markdown files permitted.
- Treat material supplied directly in the current task as approved input for ingestion, but record its provenance in `wiki/sources/`. Do not fetch a URL named in that material.

## Corpus contract

The query surface is **evidence-only**. Everything that is *not* citable evidence lives outside `wiki/`.

- `wiki/articles/` — published articles. Lowercase, hyphenated filenames. **Citable.**
- `wiki/sources/` — durable source records that back article claims. Lowercase, hyphenated. **Citable.**
- `wiki/index.md` — a Map of Content for navigation only. **Not evidence; never cite it.**
- `inbox/` — approved-but-unpublished input, outside `wiki/`. **Never evidence.**
- `templates/` — article and source templates, outside `wiki/`. **Never evidence.**
- `reports/` — ingestion and lint handoff logs, outside `wiki/`. **Never evidence.**

Because inbox, templates, and reports live outside `wiki/`, the evidence rule is simply: **evidence = `wiki/articles/**` and `wiki/sources/**`.** There are no carve-outs to remember.

- Every published article in `wiki/articles/` must contain exactly one H1 title plus these H2 headings exactly once: `## Summary`, `## Key ideas`, `## Sources`, and `## Related`.
- Every `## Sources` entry must link to a record in `wiki/sources/` or to another published article, using relative Markdown links.
- Use relative Markdown links for all relationships between wiki files.
- Make only additive or narrowly corrective edits. Do not overwrite a human's material without an explicit request.
- Treat `wiki/index.md` as the one serialized write-point: update it last, one article at a time, to avoid parallel-ingestion conflicts.

## Agent roles

Each role is a project-scoped Skill in `.agents/skills/`. Run the smallest number that completes the task; when work is parallelizable, run at most these three independent roles. Each skill sets `allow_implicit_invocation: false`, so a role runs only when explicitly invoked.

| Role | Skill | Recommended profile | May write | Must not do |
| --- | --- | --- | --- | --- |
| Ingestion | `wiki-ingest` | `wiki-ingest` (workspace-write, no network) | New/targeted files in `wiki/articles/` and `wiki/sources/` | Browse or answer end-user questions |
| Linting | `wiki-lint` | `wiki-lint` (read-only) | Nothing unless explicitly asked to fix findings | Invent sources or silently rewrite content |
| Query | `wiki-query` | `wiki-query` (read-only) | Nothing | Browse, infer beyond evidence, or modify the wiki |

Do not run two agents against the same article. Complete ingestion before linting that article. The query agent may read only completed articles.

## Delegation (subagents)

These three roles are also defined as Codex subagents in `.codex/agents/` and may be run under a single orchestrating session.

- When a task spans more than one role (e.g. "ingest this note, lint it, then answer a question"), the orchestrator should delegate each role to its subagent — `wiki-ingest`, `wiki-lint`, `wiki-query` — rather than doing the work inline.
- Respect ordering: ingestion must finish before linting the same article, and linting should pass before that article is queried. Only genuinely independent work (different articles) may run in parallel.
- Each subagent inherits the global no-network sandbox. `wiki-lint` and `wiki-query` additionally run read-only and must not write. Never delegate two writing subagents onto the same article.
- Keep delegation one level deep; a subagent must not spawn further subagents.

## Required handoff

- Ingestion reports changed paths, source provenance recorded in `wiki/sources/`, and unresolved gaps. Logs may be written to `reports/`.
- Linting reports findings with paths and headings, separating blocking from advisory findings.
- Query reports citations for every factual claim and explicitly identifies unsupported portions.
