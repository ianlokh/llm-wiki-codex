# Approved Input Inbox

Place Markdown material explicitly approved for ingestion in this folder. Files here are working input only. Because this folder lives outside `wiki/`, it is structurally excluded from the query surface and can never be used as evidence.

Use `wiki-ingest` to turn approved input into a cited page in `wiki/concepts/` or `wiki/entities/` plus a provenance record in `wiki/sources/`, then use `wiki-lint` before treating the page as queryable.
