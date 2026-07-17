# Example input: How attention transformed machine learning

> This is the worked-example article for your first ingestion run. It is safe to
> delete once you no longer need it. See `GETTING-STARTED.md` for the exercise
> and `examples/expected-result.md` for what a correct ingestion should produce.

In 2017, a team of researchers at Google published a paper with the confident
title "Attention Is All You Need". The paper, whose lead author was Ashish
Vaswani, was presented at the NeurIPS conference that year and introduced a new
neural-network architecture called the **Transformer**.

The paper's central idea is a mechanism called **self-attention**. When a model
processes a sentence, self-attention lets every word look at every other word in
the sentence and decide how much each one matters for understanding it. In the
sentence "The animal didn't cross the street because it was too tired", 
self-attention is what lets the model work out that "it" refers to the animal,
not the street.

Earlier systems for tasks like translation were built on recurrent neural
networks, which read text one word at a time, in order. The attention idea
itself predates the Transformer — Dzmitry Bahdanau and colleagues used a form of
attention to improve neural machine translation in a paper first shared in
2014 — but attention was then an add-on to a recurrent network. The Transformer's
radical move was to remove the recurrence entirely and rely on attention alone,
which allowed the computation to run in parallel and made training on large
datasets dramatically faster.

The Transformer used two refinements of the basic idea: **scaled dot-product
attention**, a formula for computing the attention weights, and **multi-head
attention**, which runs several attention operations side by side so the model
can track different kinds of relationships at once.

The results were convincing. On the WMT 2014 English-to-German translation
benchmark, the Transformer reached a BLEU score of 28.4, the best reported at
the time.

The architecture's influence went far beyond translation. Google's BERT model,
released in 2018, and OpenAI's GPT series are both built on the Transformer,
and Transformer-based models now underpin most modern large language models.
