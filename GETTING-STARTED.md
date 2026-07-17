# Getting started — build your own second brain

Welcome! This folder is a ready-made template for a **personal knowledge base**:
a private wiki that an AI assistant fills, checks, and answers questions from —
using **only what you put in it**. If the answer isn't in your wiki, the
assistant says so instead of guessing or searching the internet. That
discipline is the whole point: every answer is backed by something *you*
approved, with a clickable citation to prove it.

You do not need to know how to program. If you can download a file and click
through a permission pop-up, you can do this.

## What you need

**One** of the following:

- **Claude Desktop** (Mac or Windows) with a paid Claude plan (Pro or above).
  You'll use its **Cowork** tab, which lets Claude work inside a folder you
  choose.
- **The Codex desktop app** (from OpenAI) with a paid ChatGPT plan. It opens a
  folder as a workspace in the same way.

Optionally, a free [GitHub](https://github.com) account if you want an online
backup of your knowledge base later.

## Step 1 — Download the template

1. On the project's GitHub page, click the green **Code** button, then
   **Download ZIP**.
2. Unzip it, and move the folder somewhere you'll find it again — for example
   into your Documents folder. Rename it if you like ("my-second-brain" works).

That's it — no installation. The folder already contains everything: the rules
the assistant follows, empty shelves for your wiki pages, and a worked example
to practice on.

*(If you know git, `git clone` works too.)*

## Step 2 — Open the folder in your app

**Claude Desktop:** open the **Cowork** tab, choose this folder when asked
which folder Claude may work in, and allow access. Claude reads the rule files
in the folder automatically.

**Codex app:** open the folder as a workspace. Codex automatically reads the
folder's rule file (`AGENTS.md`) and discovers the three wiki skills.

**About permission pop-ups:** the first times the assistant reads or writes a
file, the app may ask for your approval. This is normal and good — you are the
editor-in-chief. Approving read access is always safe. For write access during
ingestion, read the pop-up and approve; it should only ever write inside this
folder.

## Step 3 — Your first ingestion

A practice article is already waiting in the `inbox/` folder (that's where
material you've approved goes before it becomes part of the wiki).

Type this to your assistant:

> Please use the wiki-ingest role to ingest the article
> `inbox/example-attention-article.md` into the wiki.

The assistant should create a **source record** (a faithful list of what the
article claims) in `wiki/sources/`, a readable **concept page** in
`wiki/concepts/`, and add one line to the wiki's table of contents,
`wiki/index.md`.

Now open [examples/expected-result.md](examples/expected-result.md) and compare.
Your wording will differ slightly — that's fine. The structure should match.

## Step 4 — Check the work

Type:

> Please use the wiki-lint role to audit the wiki.

The linter reads every page and reports problems — broken links, missing
sections, vague claims. On the example, it should report no blocking findings.
Get into the habit: **ingest, then lint, then query.**

## Step 5 — Ask your wiki a question

Type:

> Using the wiki-query role, answer only from the wiki: what BLEU score did
> the Transformer achieve, and on which benchmark?

You should get a specific answer with a clickable citation to the page it came
from. Then try a question the wiki *cannot* answer — "Who invented the
convolutional neural network?" — and you should get exactly:

> Not found in the local wiki.

That refusal is the feature you're here for. Your second brain never bluffs.

## Step 6 — Make it yours

Two small things turn the template into *your* knowledge base:

1. **Choose your subject.** Open [DOMAIN.md](DOMAIN.md) — it's the one file
   that defines what your wiki is about (its purpose, its topic tags, what
   counts as an "entity"). The shipped version targets technology and AI;
   rewrite it for your field — cooking, case law, research notes, anything.
   You never need to edit any other rule file.
2. **If you back up to GitHub:** open the file named `.gitignore` in any text
   editor and delete the clearly marked block (it starts with "⚠️ IS THIS YOUR
   OWN KNOWLEDGE BASE?"). The template ships with rules that keep *example*
   content out of the official template — but on your copy, those same rules
   would silently leave **your own pages** out of every backup. Deleting the
   block fixes that. If you never use GitHub, you can skip this.

## From here on

Your routine is simple: drop approved material (as Markdown text files) into
`inbox/`, ask for **wiki-ingest**, then **wiki-lint**, then ask your wiki
questions with **wiki-query**. Delete the example files whenever you're done
with them.

- Questions or odd behavior? See [FAQ.md](FAQ.md).
- Want the technical details (Obsidian dashboards, command-line use, how the
  roles are enforced)? See [README.md](README.md).
