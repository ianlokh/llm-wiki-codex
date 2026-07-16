---
name: wiki-lint
description: Audit the local Markdown-only wiki for structural, citation, link, and scope problems. Use when checking wiki quality without browsing, external tools, APIs, MCP resources, or model-memory supplementation.
---

# Local Wiki Linting

Read `AGENTS.md`, then inspect only `wiki/**/*.md`. Prefer the `wiki-lint` profile (read-only). Apply article checks to `wiki/articles/**/*.md` and source checks to `wiki/sources/**/*.md`; treat `wiki/index.md` as navigation, not an article.

Check every published article in `wiki/articles/` for:

1. A lowercase, hyphenated filename.
2. Exactly one H1 title and one each of `## Summary`, `## Key ideas`, `## Sources`, and `## Related`.
3. Non-empty summary, key ideas, and sources sections.
4. Every relative Markdown link resolves to an existing target.
5. Every `## Sources` entry links to a record in `wiki/sources/` or to another published article — not to an external URL presented as local evidence.
6. An entry in `wiki/index.md`.
7. Claims visibly qualified when the linked source record notes incompleteness or contradiction.
8. No template content, generated artifacts, code files, or external-source claims presented as local evidence.

Check every record in `wiki/sources/` for: a lowercase, hyphenated filename, a stated origin, and at least one asserted claim that an article can cite.

Return findings only unless explicitly asked to fix them. Label a finding **blocking** when it breaks the corpus contract or a link; otherwise **advisory**. Give each finding a file path, heading, and concise corrective action; optionally write the summary to `reports/lint-YYYY-MM-DD.md`. Do not rewrite claims or invent evidence.
