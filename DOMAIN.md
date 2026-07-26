# Wiki Domain Configuration

This file is the **one place you edit** to point this wiki at a different subject.
It is configuration, not evidence: it lives outside `wiki/`, is never cited, and is
read by the ingest, lint, and query agents in **both** apps (Codex and Claude Code).
`AGENTS.md` and `CLAUDE.md` hold the general rules that never change, and they leave
every subject-specific setting below to this file.

To point the wiki at a new subject (e.g. cooking, competitive analysis, course
notes), edit **only this file**. Everything else — the page layout, the evidence-only
rule, the `concepts/` vs `entities/` vs `sources/` folder split, the three roles, and
the dashboard/analytics queries — stays the same.

## Purpose

A closed knowledge base on **contemporary technology, applied AI/ML, and the
companies and products shaping them** — spanning ML foundations, consumer and
enterprise hardware/software, telecom, healthcare AI, and security. Pages capture
durable, evidence-backed understanding of how things work (concepts) and of the
specific named things involved (entities), including forward-looking product
rumors recorded with appropriate confidence.

## Tag taxonomy

The `tags:` page-metadata field is a **subject index that cuts across folders** — it
is separate from the folder, which is what marks a page as a concept, entity, or
source. A page may carry several tags, and the same tag can appear on both
`concepts/` and `entities/` pages. Tags describe subject only — never use a tag to
record confidence or claim status (use the `confidence` field for that).

This list is a **controlled list that can grow carefully**, not a locked cage. While
ingesting, the agent picks tags from the page content, and:

- **reuses** an existing tag below whenever one reasonably fits (prefer reusing over
  inventing near-duplicates — `apple` not `apple-inc`, `ml-foundations` not `ml`);
- if nothing really fits, **adds a new tag to this list** as part of the same
  ingest — a one-line change you can review — rather than quietly using a tag that is
  not on the list.

The linter treats a tag that is not on this list as **advisory** (it flags it but
does not block), so stray tags show up instead of quietly splitting the
dashboard/analytics groupings. Keep the list short and tidy; the entries below are a
starting set, not a limit.

- `ml-foundations` — core ML/DL ideas, algorithms, and teaching implementations
- `ai-systems` — model architectures, training/inference infrastructure, autograd
- `ai-applications` — applied AI in a specific vertical or workflow
- `consumer-hardware` — phones, wearables, smart-home, chips for consumer devices
- `software-platforms` — operating systems, OS releases, developer platforms
- `telecom-networking` — networks, RAN, connectivity infrastructure
- `healthcare` — clinical, compliance, and life-sciences applications
- `security` — malware, threats, defensive and offensive security
- `enterprise-ai` — AI adoption inside businesses and product development
- `apple` — the Apple ecosystem (cross-cuts hardware/software; org-specific)
- `research` — research programs, papers, and scientific initiatives
- `ai-industry` — AI labs/companies, funding, IPOs, product launches, and industry moves
- `ai-policy` — AI regulation, legislation, government programs, and public policy
- `ai-safety` — AI alignment, scheming/misalignment, model welfare, and safety research proposals

## Confidence rubric

The `confidence:` page-metadata field is the whole page's summary of the claim-status
wording the corpus contract already tracks on each claim (reported / rumored /
confirmed / fact). Choose the level that matches the *least-certain claim the page
actually relies on*.

- `high`   — established fact or officially confirmed; multiple or authoritative
             sources; shipping products, published methods, settled history.
- `medium` — reported by a credible source but not officially confirmed;
             single-sourced reporting; announced-but-not-yet-shipped.
- `low`    — rumored, speculative, or forward-looking; unverified leaks,
             predicted products, contested claims.

## Entity scope

An **entity** page describes one specific named thing — a name that could carry its
own fact-box. The `entity_type` frontmatter field records which kind; its controlled
vocabulary for this domain is:

- **product** — a device, app, or release (e.g. macOS 27 Golden Gate, foldable iPhone)
- **organization** — a company, team, or institution (e.g. Apple, Nokia)
- **tool** — a named implementation or software tool
- **model** — a named model (e.g. Micrograd)
- **platform** — a named platform (e.g. Nokia AI-RAN Platform)
- **initiative** — a named program or initiative
- **threat** — a named malware family or campaign (e.g. CrashStealer)
- **person** — a named individual
- **place** — a named location

## Page metadata (frontmatter)

Every published page carries a YAML frontmatter block. The **set of keys is the same
in every clone** (defined once in `templates/README.md`); this section only lists the
**values you can change per subject**. To retarget the wiki, edit the settings below
along with the tag list and confidence levels above.

**`origin_kind`** — the allowed values for the shape of a source record's input:

- `article` — a single article or post
- `digest` — a multi-story digest or newsletter
- `feed` — a homepage or feed snapshot
- `report` — a multi-finding report or white paper
- `dataset` — structured/tabular data
- `document` — a standalone document (spec, filing, memo)

**Enabled optional fields** — which non-essential fields this domain fills in. Because
this domain records forward-looking rumors and links between pages, all three are on:

- `confidence` — **enabled** (concept/entity); uses the rubric above.
- `aliases` — **enabled**; acronyms and alternate names.
- `related` — **enabled**; associative links to other pages.

**`link_style`** — `slug` (the default): link fields like `related` hold the bare
file name (the filename stem, without `.md`), which keeps the wiki portable and
Markdown-only. A clone that lives entirely in Obsidian may set this to `wikilink` and
use `"[[Title]]"` values instead, giving up portability for Obsidian's built-in graph
links.

**Custom fields** — clones may add their own domain-specific keys (e.g. `prep_time`
for a cooking wiki); list the ones you use regularly here so the vocabulary stays
visible. Linting treats unknown keys as advisory, never blocking.

## Concept scope

A **concept** page explains *how an idea or method works* or *what a topic means* —
not the details of one single named thing. In this domain, concepts include
algorithms and methods (backpropagation), strategies and patterns (AI chip
acquisition strategy), and applied approaches (AI for hospital 340B compliance,
AI-assisted product development). When genuinely unsure, put topics in `concepts/` and
named proper nouns in `entities/`.
