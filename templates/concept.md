---
# Frontmatter schema — see templates/README.md for rules, DOMAIN.md for allowed values.
title:                      # human-readable, matches the H1 below
type: concept               # fixed for this template (= folder)
tags: []                    # DOMAIN.md tag registry (open axis; reuse-first)
confidence:                 # high | medium | low — DOMAIN.md confidence rubric
created:                    # YYYY-MM-DD (ISO 8601, unquoted)
updated:                    # YYYY-MM-DD
summary:                    # one-line abstract (≤ ~200 chars) for dashboards
aliases: []                 # optional: acronyms / alternate names
related: []                 # optional: bare slugs of related pages (link_style: slug)
---

# Title

## Summary

Write a concise, evidence-backed overview of the idea, method, or topic.

## Key ideas

- State one supported idea per bullet. A concept page explains *how something works* or *what an idea means* — not the properties of a single named thing (use an entity page for that).

## Sources

- Link each claim's provenance to a record in `wiki/sources/` using a relative Markdown link, e.g. `[source: attention-is-all-you-need](../sources/attention-is-all-you-need.md)`. Do not cite external URLs directly.

## Related

- Add relative links to other published concept or entity pages, or write `None yet.`
