---
# Frontmatter schema — see templates/README.md for rules, DOMAIN.md for allowed values.
title:                      # human-readable, matches the H1 below
type: source                # fixed for this template (= folder)
origin_kind:                # article|digest|feed|report|dataset|document — DOMAIN.md
source_format:              # markdown|pdf|html|url|text
tags: []                    # from the DOMAIN.md tag list (open-ended; reuse first)
item_count:                 # integer: distinct substantive items in the input
claim_count:                # integer: bullets under ## What it asserts
summary:                    # one-line abstract of the source (optional)
ingested:                   # YYYY-MM-DD (ISO 8601, unquoted)
ingested_by: wiki-ingest
---

# Source: Title

## Origin

Describe exactly what this material is and where it came from within the approved input (e.g. the inbox filename, or "supplied directly in task on 2026-07-16"). Do not record a live URL as something to fetch. State the input's size and shape (e.g. "a single article", "a 17-story news digest", "a 200-row dataset") so a reader — and linting, which cannot read the original input — can see how much should be covered.

## Ingested

- Date ingested:
- Ingested by role: `wiki-ingest`

## What it asserts

- List the specific claims this source supports, each one able to be cited on its own — one bullet per separate, meaningful item in the input, keeping names, dates, quantities, who said it, and status (reported/rumored/confirmed/fact). Pages cite these. Do not collapse a multi-item input into topic labels ("covers hardware rumors, pricing, and litigation"), and leave out filler (ads, navigation, markup).

## Limitations

- Note gaps, uncertainty, or contradictions in the material so pages can qualify claims honestly.
