# Frequently asked questions

### Why won't it answer from the internet? It clearly knows more than my wiki.

By design. This wiki is **closed-world**: answers come only from pages you
deliberately added, each backed by a cited source record. The moment the
assistant fills gaps from the internet or its own training, you can no longer
trust that an answer reflects *your* vetted material. If you want general
knowledge, ask in a normal chat; if you want *your* knowledge, ask the wiki.

One honest caveat: the rules instruct the assistant not to browse, and the
wiki roles are set up without web tools — but on personal plans, neither
desktop app lets you fully switch off the app's own web-search feature. If an
answer ever looks like it came from the web, say "answer only from the wiki
using wiki-query" — invoking the role by name applies the strictest rules.

### It answered "Not found in the local wiki." What now?

That's the system working — the wiki genuinely doesn't contain support for an
answer. If you want it answered in future: find material that covers it, check
it, drop it in `inbox/`, and run **wiki-ingest**. Then ask again.

### I backed up to GitHub, but none of my pages are there!

The template ships with a file called `.gitignore` that deliberately keeps
example content out of the official template — and on your copy, the same
rules exclude **your** pages from backup. Open `.gitignore` in any text editor
and delete the clearly marked block (see
[GETTING-STARTED.md](GETTING-STARTED.md), step "Make it yours"). Then back up
again — your pages will be included. Your files were never deleted; they were
only being skipped by the backup.

### The assistant asks for permission constantly. Is something wrong?

No — the app is being careful with your files, and you are meant to be the
gatekeeper. Approving *read* access is always safe. For *write* access, a
glance at the pop-up is enough: it should only touch files inside your wiki
folder. Both apps have a setting to reduce how often they ask once you're
comfortable (in Claude's Cowork, the approval mode; in the Codex app, the
permissions selector under the message box).

### How do I change what my wiki is about?

Edit one file: [DOMAIN.md](DOMAIN.md). It defines the wiki's purpose, its
topic tags, its confidence scale, and what counts as a concept versus an
entity. Rewrite it for your subject and the three roles follow it
automatically. Don't edit the other rule files (`AGENTS.md`, `CLAUDE.md`) —
they hold the universal machinery.

### What are wiki-ingest, wiki-lint, and wiki-query, in one line each?

- **wiki-ingest** — the librarian: turns approved material from `inbox/` into
  wiki pages plus source records. The only role allowed to write.
- **wiki-lint** — the inspector: reads everything, reports problems, changes
  nothing.
- **wiki-query** — the reference desk: answers questions from the wiki only,
  with a citation for every claim.

Always in that order: ingest, then lint, then query.

### Is my knowledge base private?

The wiki is plain text files in a folder on your computer — no database, no
cloud service of its own. You can read every file with any text editor, and it
also opens directly as an [Obsidian](https://obsidian.md) vault. Note that
whatever you *show* the assistant is processed by the AI service you use
(Anthropic or OpenAI) under that service's privacy terms, like any chat. If
you back up to GitHub, use a **private** repository unless you want the
content public.

### Can I use both Claude and Codex on the same wiki?

Yes — that's a design goal. Both apps read the same folder, follow equivalent
rules (`CLAUDE.md` for Claude, `AGENTS.md` for Codex), and produce the same
page format. You can ingest with one and query with the other.
