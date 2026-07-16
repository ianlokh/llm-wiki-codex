# Local Wiki Operating Rules (Claude Code)

This repository is a closed, Markdown-only knowledge base. Treat `wiki/**/*.md` as the only authority for factual answers. This file mirrors `AGENTS.md`; the two must stay in sync so the wiki behaves identically under Codex and Claude Code.

## Non-negotiable boundaries

- Do not browse the web, call external services, or consult any information outside this repository while ingesting, linting, or querying.
- Enforcement here is by **tool gating**: the role subagents in `.claude/agents/` are granted no web tools, and the lint/query roles are granted no write tools. Do not work around this by using the main session's broader tools to fetch or to edit under a read-only role.
- Do not fill gaps with model memory, general knowledge, or inference. If the corpus does not support an answer, say exactly: `Not found in the local wiki.`
- Cite every substantive answer with a local path and heading, e.g. `wiki/articles/attention.md#Key ideas`.
- Keep the repository Markdown-only. TOML/YAML under `codex/`, `.codex/agents/`, and `.agents/skills/**/agents/`, plus the Claude Code config under `.claude/` (`settings.json` and the agent/skill definitions), are configuration, not code. Codex-provisioned system tooling under `skills/.system/` is machine-managed and git-ignored, not project content.

## Corpus contract

The query surface is **evidence-only**; everything that is not citable evidence lives outside `wiki/`.

- `wiki/articles/` — published articles (lowercase, hyphenated). **Citable.**
- `wiki/sources/` — durable source records backing article claims. **Citable.**
- `wiki/index.md` — Map of Content, navigation only. **Never cite it.**
- `inbox/`, `templates/`, `reports/` — outside `wiki/`. **Never evidence.**

So the rule is simply: **evidence = `wiki/articles/**` and `wiki/sources/**`.**

- Every published article has exactly one H1 plus one each of `## Summary`, `## Key ideas`, `## Sources`, `## Related`.
- Every `## Sources` entry links to a record in `wiki/sources/` or another article, via relative Markdown links.
- Make only additive or narrowly corrective edits. Update `wiki/index.md` last, one article at a time.

## Roles and delegation (subagents)

Three subagents in `.claude/agents/` mirror the Codex roles. Invoke them with the Task tool; do the work through them rather than inline when a task spans roles.

| Role | Subagent | Tools granted | Enforces |
| --- | --- | --- | --- |
| Ingestion | `wiki-ingest` | Read, Write, Edit, Glob, Grep | Can write the corpus; no web |
| Linting | `wiki-lint` | Read, Glob, Grep | Read-only; reports findings |
| Query | `wiki-query` | Read, Glob, Grep | Read-only; wiki-only answers |

Ordering: ingestion must finish before linting the same article, and linting should pass before that article is queried. Only independent articles may be processed in parallel. Never point two writing subagents at the same article. Subagents must not spawn further subagents.

## Skills

`.claude/skills/{wiki-ingest,wiki-lint,wiki-query}/SKILL.md` are symlinks to the canonical skills in `.agents/skills/` — one source of truth shared with Codex. Editing either path edits the same file.
