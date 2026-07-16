# Backpropagation

## Summary

Backpropagation is an algorithm for computing the gradients of a scalar loss with respect to the parameters of a function, such as a neural network, by applying the chain rule of calculus. It runs in two phases: a forward pass that computes the output and caches intermediate values, and a backward pass that propagates gradients from the output back toward the inputs. Backpropagation is a specific instance of reverse-mode automatic differentiation, and its efficiency comes from reusing shared intermediate results so that all parameter gradients are obtained in a single backward pass.

## Key ideas

- Backpropagation computes the gradients of a scalar loss with respect to the parameters of a function (such as a neural network) by applying the chain rule of calculus.
- It runs in two phases: a forward pass that computes the output and caches intermediate values, and a backward pass that propagates gradients from the output back toward the inputs.
- It is a specific instance of reverse-mode automatic differentiation.
- The backward pass visits nodes in reverse topological order, multiplying local derivatives together along the way (the chain rule).
- Its efficiency comes from reusing shared intermediate results, so all parameter gradients are obtained in a single backward pass rather than one pass per parameter.
- micrograd is described as a minimal educational implementation of backpropagation over scalar values.

## Sources

- [source: backpropagation](../sources/backpropagation.md)

## Related

- [Micrograd](micrograd.md)
