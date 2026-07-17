# Expected result of the worked example

This file shows what a correct ingestion of `inbox/example-attention-article.md`
should produce, so you can compare your first run against a known-good answer.
It lives outside `wiki/`, so it is never evidence and is never cited. Small
wording differences from your own run are fine — what matters is the structure:
one source record, one concept page, one index entry, and specific
(not vague) claims.

Your run should create **two new files** and **update one**:

| File | What it is |
| --- | --- |
| `wiki/sources/example-attention-article.md` | The source record — the faithful list of what the article asserts |
| `wiki/concepts/attention.md` | The concept page — the readable article built on that evidence |
| `wiki/index.md` | Gains one line linking the new concept page |

---

## 1. The source record — `wiki/sources/example-attention-article.md`

```markdown
---
title: "Example input: How attention transformed machine learning"
type: source
origin_kind: article
source_format: markdown
tags: [ml-foundations, ai-systems]
item_count: 1
claim_count: 8
summary: A short explainer article on the 2017 Transformer paper and the attention mechanism.
ingested: 2026-07-18
ingested_by: wiki-ingest
---

# Source: Example input: How attention transformed machine learning

## Origin

A single short explainer article supplied as `inbox/example-attention-article.md`
(the worked example shipped with this template). One substantive item: the
history and mechanics of attention and the Transformer architecture.

## Ingested

- Date ingested: 2026-07-18
- Ingested by role: `wiki-ingest`

## What it asserts

- The paper "Attention Is All You Need", lead author Ashish Vaswani, was published by a team at Google and presented at NeurIPS 2017 (established fact).
- The paper introduced the Transformer neural-network architecture (established fact).
- Self-attention lets every word in a sentence weigh the relevance of every other word, e.g. resolving what "it" refers to (established fact).
- A form of attention was used earlier than the Transformer: Dzmitry Bahdanau and colleagues applied it to neural machine translation in a paper first shared in 2014, as an add-on to a recurrent network (established fact).
- The Transformer removed recurrence entirely and relied on attention alone, allowing parallel computation and much faster training on large datasets (established fact).
- The Transformer used scaled dot-product attention and multi-head attention, which runs several attention operations side by side (established fact).
- The Transformer reached a BLEU score of 28.4 on the WMT 2014 English-to-German benchmark, the best reported at the time (established fact).
- Google's BERT (2018) and OpenAI's GPT series are built on the Transformer, which now underpins most modern large language models (established fact).

## Limitations

- A secondary explainer, not the original paper; technical detail is simplified.
- Single-source input: no independent corroboration recorded in this wiki.
```

**What to check in your version:** the date should be the day *you* ran it; the
claims keep names, dates, and quantities (Vaswani, 2017, 28.4 BLEU) instead of
vague labels like "covers the history of attention"; `claim_count` matches the
number of bullets.

---

## 2. The concept page — `wiki/concepts/attention.md`

```markdown
---
title: Attention
type: concept
tags: [ml-foundations, ai-systems]
confidence: high
created: 2026-07-18
updated: 2026-07-18
summary: The neural-network mechanism that lets each token weigh all others, and the basis of the Transformer architecture.
aliases: [self-attention]
related: []
---

# Attention

## Summary

Attention is a neural-network mechanism that lets a model weigh how relevant
every element of an input is to every other element. Introduced as an add-on to
recurrent translation models, it became the sole foundation of the Transformer
architecture in 2017, which now underpins most modern large language models.

## Key ideas

- Self-attention lets every word in a sentence assess the relevance of every other word, which is how a model resolves references such as what "it" points to.
- Attention predates the Transformer: Dzmitry Bahdanau and colleagues used it in 2014 to improve neural machine translation, attached to a recurrent network.
- The 2017 paper "Attention Is All You Need" (Vaswani et al., Google, NeurIPS 2017) introduced the Transformer, which removed recurrence entirely and relies on attention alone, enabling parallel computation and faster training.
- The Transformer refines the basic idea with scaled dot-product attention and multi-head attention, which runs several attention operations side by side.
- The Transformer set a then-best BLEU score of 28.4 on the WMT 2014 English-to-German benchmark, and became the foundation of BERT (Google, 2018) and OpenAI's GPT series.

## Sources

- [source: example-attention-article](../sources/example-attention-article.md)

## Related

- None yet.
```

**What to check in your version:** exactly one H1 and the four required `##`
sections; `confidence: high` (every load-bearing claim is an established fact);
every claim in Key ideas traceable to a bullet in the source record; the
Sources entry links to the record with a relative link.

---

## 3. The index entry — added to `wiki/index.md`

Under `## Concepts`, replacing the "*No pages yet*" placeholder:

```markdown
- [Attention](concepts/attention.md)
```

---

## After ingesting: lint, then query

Running `wiki-lint` on this result should report **no blocking findings**.
Then try two queries with `wiki-query`:

1. *"What BLEU score did the Transformer achieve, and on which benchmark?"* —
   should answer 28.4 on WMT 2014 English-to-German, with a clickable citation
   like `[wiki/concepts/attention.md — Key ideas](wiki/concepts/attention.md)`.
2. *"Who invented the convolutional neural network?"* — should answer exactly:
   `Not found in the local wiki.` That refusal is the closed-world design
   working, not a failure.
