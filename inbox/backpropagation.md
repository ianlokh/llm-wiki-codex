# Backpropagation — approved input

Material approved for ingestion on 2026-07-16. Use only what is stated here.

- Backpropagation is an algorithm for computing the gradients of a scalar loss with respect to the parameters of a function (such as a neural network) by applying the chain rule of calculus.
- It runs in two phases: a forward pass that computes the output and caches intermediate values, and a backward pass that propagates gradients from the output back toward the inputs.
- It is a specific instance of reverse-mode automatic differentiation.
- The backward pass visits nodes in reverse topological order, multiplying local derivatives together along the way (the chain rule).
- Its efficiency comes from reusing shared intermediate results, so all parameter gradients are obtained in a single backward pass rather than one pass per parameter.
- micrograd is a minimal educational implementation of backpropagation over scalar values.
