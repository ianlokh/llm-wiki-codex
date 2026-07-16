# Karpathy Wiki

A local-first, Markdown-only knowledge base operated through Codex. The wiki is intentionally closed-world: a missing answer is reported as missing, not supplemented from the internet or model knowledge.

## One-time setup (registration)

Codex loads configuration from `$CODEX_HOME` (default `~/.codex/`), **not** from this repository. So the TOML under `codex/` is a set of templates you apply once:

1. **Enforce closed-world + register skills.** Merge `codex/config.sample.toml` into `~/.codex/config.toml` (it sets `sandbox_workspace_write.network_access = false` and pins the three skills to `enabled = true`).
2. **Install the per-role profiles.** Copy each file in `codex/profiles/` into `$CODEX_HOME` keeping the name pattern, e.g. `cp codex/profiles/wiki-query.config.toml ~/.codex/wiki-query.config.toml`.

The project-local parts — `AGENTS.md` and `.agents/skills/` — are auto-loaded by Codex and need no installation.

## Use it

Open this folder as a Codex project. Codex reads `AGENTS.md` first, then you explicitly invoke one of the three Markdown-only skills, ideally with its matching profile:

- Ingest approved material: `codex --profile wiki-ingest` → invoke `wiki-ingest`.
- Audit the corpus: `codex --profile wiki-lint` → invoke `wiki-lint`.
- Ask a local-only question: `codex --profile wiki-query` → invoke `wiki-query`.

The query and lint profiles use a `read-only` sandbox, so those roles *cannot* modify the wiki. The ingest profile allows writes but keeps the network unreachable.

## Layout

```
AGENTS.md              Enforced working contract for all three roles
wiki/                  EVIDENCE ONLY — the entire query surface
  index.md             Map of Content (navigation, never cited)
  articles/            Published, cited articles (citable)
  sources/             Durable source records backing article claims (citable)
inbox/                 Approved-but-unpublished input (never evidence)
templates/             Article + source templates (never evidence)
reports/               Ingest/lint handoff logs (never evidence)
.agents/skills/        The three Markdown-only skills (auto-discovered)
codex/                 TOML templates to merge into $CODEX_HOME
```

## Corpus flow

Place approved Markdown source material in `inbox/`. Use `wiki-ingest` to convert it into an evidence-backed article in `wiki/articles/` plus a provenance record in `wiki/sources/`, then use `wiki-lint` before querying it with `wiki-query`.
