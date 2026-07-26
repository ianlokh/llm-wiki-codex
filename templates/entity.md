---
# Frontmatter schema — see templates/README.md for rules, DOMAIN.md for allowed values.
title:                      # human-readable, matches the H1 below
type: entity                # fixed for this template (= folder)
entity_type:                # product|organization|tool|model|platform|initiative|threat|person|place — DOMAIN.md entity scope
tags: []                    # from the DOMAIN.md tag list (open-ended; reuse first)
confidence:                 # high | medium | low — DOMAIN.md confidence rubric
created:                    # YYYY-MM-DD (ISO 8601, unquoted)
updated:                    # YYYY-MM-DD
summary:                    # one-line summary (≤ ~200 chars) for dashboards
aliases: []                 # optional: acronyms / alternate names
related: []                 # optional: bare slugs of related pages (link_style: slug)
---

# Title

## Summary

Write a concise, evidence-backed overview of the entity — a person, organization, product, tool, model, or place.

## Key ideas

- State one supported fact per bullet. An entity page describes *a specific named thing*: what it is, who made it, what it does, how it relates to other entities and concepts.

## Sources

- Link each claim to where it came from — a record in `wiki/sources/` — using a relative Markdown link, e.g. `[source: micrograd](../sources/micrograd.md)`. Do not cite external URLs directly.

## Related

- Add relative links to other published concept or entity pages, or write `None yet.`
