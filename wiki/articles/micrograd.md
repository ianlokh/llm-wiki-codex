# Micrograd

## Summary

micrograd is a tiny automatic differentiation (autograd) engine written by Andrej Karpathy. It implements reverse-mode automatic differentiation — that is, backpropagation — over a dynamically built directed acyclic graph (DAG) of scalar values. The autograd engine is roughly 100 lines of Python, with a small neural-network library built on top of it. Its purpose is educational: to show that backpropagation is just the repeated application of the chain rule, not something magical. Because it operates on individual scalars rather than tensors, it trades runtime efficiency for conceptual clarity.

## Key ideas

- micrograd is a tiny autograd (automatic differentiation) engine written by Andrej Karpathy.
- It performs reverse-mode automatic differentiation — i.e. backpropagation — over a dynamically built directed acyclic graph (DAG).
- The graph is built from scalar values rather than tensors; each node is a `Value` object storing its data, its gradient, references to the child nodes that produced it, and the operation used.
- Calling `.backward()` on an output node topologically sorts the graph and applies the chain rule in reverse to populate `.grad` on every node.
- The autograd engine is roughly 100 lines of Python, with a small neural-network library built on top of it.
- Its purpose is educational: to show that backpropagation is just the repeated application of the chain rule.
- Operating on individual scalars rather than tensors trades runtime efficiency for conceptual clarity.

## Sources

- [source: micrograd](../sources/micrograd.md)

## Related

None yet.
