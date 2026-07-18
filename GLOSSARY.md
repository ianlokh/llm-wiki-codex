# Glossary — plain-language definitions

Every term this project uses, explained in one line each. If a rule file or a skill
uses a word you don't recognise, look it up here first. Nothing on this page is a
rule — it's just a dictionary.

## The big idea

- **Knowledge base / wiki** — a private collection of pages, stored as plain text
  files on your computer, that an AI assistant fills in and answers questions from.
- **Closed-world** — the assistant answers *only* from the pages in your wiki. If the
  answer isn't there, it says so instead of guessing or searching the web.
- **Evidence** — the pages the assistant is allowed to quote when it answers. Only
  three folders count as evidence: `concepts/`, `entities/`, and `sources/`.
- **Corpus** — a single word for "all the pages in your wiki, taken together."

## The kinds of pages

- **Concept page** — explains *how an idea or method works* (for example
  "emulsification" or "backpropagation"). Lives in `wiki/concepts/`.
- **Entity page** — describes *one specific named thing*: a person, company, product,
  tool, model, or place (for example "Apple" or "iPhone"). Lives in `wiki/entities/`.
- **Source record** — a faithful list of everything a piece of input actually said,
  kept so every claim on a concept or entity page can point back to where it came
  from. Lives in `wiki/sources/`.
- **Provenance** — the record of *where a fact came from*. A source record is how this
  wiki keeps provenance.
- **Map of Content (`wiki/index.md`)** — the wiki's table of contents. Its job is
  finding pages, so it is *never* used as evidence and is never cited.

## The three jobs (roles)

- **Ingest (`wiki-ingest`)** — the librarian. Turns approved input into wiki pages
  plus a source record. The only role allowed to *write* files.
- **Lint (`wiki-lint`)** — the inspector. Reads every page and reports problems
  (broken links, missing sections, vague claims). Changes nothing.
- **Query (`wiki-query`)** — the reference desk. Answers your questions using the wiki
  only, with a citation for every claim.
- Always run them in this order: **ingest, then lint, then query.**
- **Role / agent / skill / subagent** — these words all point at the three jobs above.
  A *skill* is the written instructions for a job; an *agent* (or *subagent*) is the
  assistant doing that job while following those instructions.

## Page anatomy

- **Frontmatter** — a small block of labelled facts at the very top of a page, fenced
  by `---` lines (title, tags, date, and so on). It is *about* the page and is never
  quoted as evidence.
- **YAML** — the simple `key: value` format the frontmatter is written in.
- **Field / key** — one labelled line inside the frontmatter, such as `title:` or
  `tags:`.
- **Heading** — a line starting with `#`. One `#` is an **H1** (the page title, one
  per page); two `##` is an **H2** (a section such as `## Summary`).
- **Tag** — a subject label in the frontmatter (like `security` or `apple`) used to
  group related pages. Chosen from the list in `DOMAIN.md`.
- **Confidence** — a frontmatter label (`high` / `medium` / `low`) saying how solid a
  page's facts are: confirmed vs. reported vs. rumored.
- **Slug** — the short, lowercase-with-hyphens file name of a page, without the `.md`
  (for example `attention.md` → `attention`).
- **Citation** — a clickable link in an answer that points to the exact page a fact
  came from.

## Words you'll meet in the rule files

- **Ingestion / to ingest** — the act of turning approved input into wiki pages.
- **Decompose** — break a piece of input into its separate individual facts *before*
  summarising it, so no detail is lost.
- **Aggregation** — a single input that bundles many separate items (a newsletter, a
  feed, a multi-finding report, a spreadsheet).
- **Blocking vs. advisory** — the two levels of problem the inspector reports.
  *Blocking* = must be fixed. *Advisory* = worth a look, but not required.
- **Additive / corrective edit** — only *add* new material or *gently fix* an error;
  don't rewrite or delete what a person wrote unless you're asked.
- **Retarget** — point the whole wiki at a new subject (from tech to cooking, say) by
  editing one file, `DOMAIN.md`.
- **Controlled vocabulary / registry** — a fixed list of allowed values (such as the
  tag list or the confidence levels) that pages must choose from.
- **Read-only** — a role that can look at files but cannot change them. The inspector
  and reference-desk roles are read-only; no role can browse the web.
- **Runtime** — the app you run the wiki in. This template supports two: Claude Code
  (which reads `CLAUDE.md`) and Codex (which reads `AGENTS.md`).

## For the technical setup only

You can skip these unless you're wiring up backups or using the command line.

- **Repository / repo** — the project folder, tracked by the Git version-control tool.
- **Sandbox** — a locked-down mode (used by Codex) that makes the internet physically
  unreachable while a role runs.
- **Tool-gating** — handing each role only the tools it needs, so the inspector and
  reference-desk roles literally *cannot* write files and no role can reach the web.
- **Mirror / byte-identical** — two copies of a file that must match exactly. The
  Claude skill files under `.claude/skills/` are exact copies of the originals under
  `.agents/skills/`; an automatic check fails if they drift apart.
- **CI workflow** — an automatic check that runs when you upload to GitHub; here it
  only guards the mirror copies above.
