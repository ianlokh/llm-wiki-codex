# Wiki Domain Configuration

This file is the **single customization surface** for retargeting this wiki to a
different subject area. It is configuration, not evidence: it lives outside `wiki/`,
is never cited, and is read by the ingest/lint/query agents in **both** runtimes
(Codex and Claude Code) via Read/Grep. `AGENTS.md` and `CLAUDE.md` hold the
universal, domain-agnostic operating rules and defer to this file for every
domain-specific knob below.

To retarget the wiki (e.g. to cooking, competitive analysis, course notes), edit
**only this file**. Everything else — the page schema, the evidence-only corpus
contract, the `concepts/` vs `entities/` vs `sources/` folder split, the tool-gated
roles, and the dashboard/analytics queries — stays universal and unchanged.

## Purpose

A closed knowledge base on **contemporary technology, applied AI/ML, and the
companies and products shaping them** — spanning ML foundations, consumer and
enterprise hardware/software, telecom, healthcare AI, and security. Pages capture
durable, evidence-backed understanding of how things work (concepts) and of the
specific named things involved (entities), including forward-looking product
rumors recorded with appropriate confidence.

## Tag taxonomy

The `tags:` page-metadata field is a **cross-cutting subject index** — orthogonal
to the folder, which carries the concept/entity/source distinction instead. A page
may hold several tags, and the same tag spans both `concepts/` and `entities/`.
Tags are subject facets only — never encode confidence or claim status as a tag
(use the `confidence` field for that).

This vocabulary is a **governed-growth registry**, not a fixed cage. During
ingestion the agent selects tags on the fly from the page content, and:

- **reuses** an existing tag below whenever one reasonably fits (prefer reuse over
  minting near-synonyms — `apple` not `apple-inc`, `ml-foundations` not `ml`);
- if genuinely nothing fits, **proposes a new tag by adding it to this list** as
  part of the same ingest — a one-line, reviewable edit — rather than inventing an
  off-list tag silently.

Lint treats off-registry tags as **advisory** (flag, do not block), so drift
becomes visible instead of silently fragmenting the dashboard/analytics grouping.
Keep the list small and curated; the entries below are the seed vocabulary, not a
ceiling.

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

## Confidence rubric

The `confidence:` page-metadata field is the page-level rollup of the claim-status
language the corpus contract already tracks per claim (reported / rumored /
confirmed / fact). Choose the level matching the *weakest load-bearing* claim on
the page.

- `high`   — established fact or officially confirmed; multiple or authoritative
             sources; shipping products, published methods, settled history.
- `medium` — reported by a credible source but not officially confirmed;
             single-sourced reporting; announced-but-not-yet-shipped.
- `low`    — rumored, speculative, or forward-looking; unverified leaks,
             predicted products, contested claims.

## Entity scope

An **entity** page describes one specific named thing — a proper noun that could
carry an infobox. The `entity_type` frontmatter field records which kind; its
controlled vocabulary for this domain is:

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

Every published page carries a YAML frontmatter block. The **key set is universal**
(defined once in `templates/README.md` and the same in every clone); this section
holds only the **domain-configurable values**. Retarget the wiki by editing the
knobs below alongside the tag registry and confidence rubric above.

**`origin_kind`** — controlled vocabulary for a source record's input shape:

- `article` — a single article or post
- `digest` — a multi-story digest or newsletter
- `feed` — a homepage or feed snapshot
- `report` — a multi-finding report or white paper
- `dataset` — structured/tabular data
- `document` — a standalone document (spec, filing, memo)

**Enabled optional fields** — which non-core fields this domain populates. Because
this domain records forward-looking rumors and relationships, all three are on:

- `confidence` — **enabled** (concept/entity); uses the rubric above.
- `aliases` — **enabled**; acronyms and alternate names.
- `related` — **enabled**; associative links to other pages.

**`link_style`** — `slug` (default): relational fields (`related`) hold bare
filename-stem slugs, keeping the corpus portable and Markdown-only. A clone that
lives entirely in Obsidian may set this to `wikilink` and use `"[[Title]]"` values
instead, trading portability for native graph resolution.

**Custom fields** — clones may add domain-specific keys (e.g. `prep_time` for a
cooking wiki); declare recurring ones here so the vocabulary stays visible. Linting
treats unknown keys as advisory, never blocking.

## Concept scope

A **concept** page explains *how an idea or method works* or *what a topic means* —
not the properties of a single named thing. In this domain, concepts include
algorithms and methods (backpropagation), strategies and patterns (AI chip
acquisition strategy), and applied approaches (AI for hospital 340B compliance,
AI-assisted product development). When genuinely unsure, prefer `concepts/` for
topics and `entities/` for named proper nouns.

## Journal focus (optional)

Research-journal entries under `wiki/journal/` are dated working logs of ingestion
sessions, investigations, and open questions in this domain. They are **never
evidence and never cited**; a durable fact discovered in the journal must be
promoted into a `wiki/sources/` record before it can back a page.
