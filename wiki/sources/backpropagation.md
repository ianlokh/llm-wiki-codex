# Source: Backpropagation — approved input

## Origin

From `inbox/backpropagation.md`, material approved for ingestion on 2026-07-16. The file states: "Use only what is stated here." No external URL, author, or publication is identified in the material.

## Ingested

- Date ingested: 2026-07-16
- Ingested by role: `wiki-ingest`

## What it asserts

- Backpropagation is an algorithm for computing the gradients of a scalar loss with respect to the parameters of a function (such as a neural network) by applying the chain rule of calculus.
- It runs in two phases: a forward pass that computes the output and caches intermediate values, and a backward pass that propagates gradients from the output back toward the inputs.
- It is a specific instance of reverse-mode automatic differentiation.
- The backward pass visits nodes in reverse topological order, multiplying local derivatives together along the way (the chain rule).
- Its efficiency comes from reusing shared intermediate results, so all parameter gradients are obtained in a single backward pass rather than one pass per parameter.
- micrograd is a minimal educational implementation of backpropagation over scalar values.

## Limitations

- The material provides no attribution (no author, publication, or date of the underlying claims) beyond the inbox file itself.
- It gives no worked example, equations, or pseudocode.
- It does not cover numerical considerations (e.g. vanishing/exploding gradients), memory cost of caching intermediates, or how backpropagation differs from forward-mode automatic differentiation beyond naming it as reverse-mode.
- It does not detail how backpropagation applies to non-scalar (tensor) computations.
