# Source: Micrograd — approved input

## Origin

Ingested from the approved input file `inbox/micrograd.md`, headed "Micrograd — approved input". The material was approved for ingestion on 2026-07-16 and states it summarizes the micrograd project, with the instruction to use only what is stated there. No external URL was recorded or fetched.

## Ingested

- Date ingested: 2026-07-16
- Ingested by role: `wiki-ingest`

## What it asserts

- micrograd is a tiny automatic differentiation (autograd) engine written by Andrej Karpathy.
- It implements reverse-mode automatic differentiation — i.e. backpropagation — over a dynamically built directed acyclic graph (DAG).
- The graph is built from scalar values, not tensors. Each node is a `Value` object storing its data, its gradient, references to the child nodes that produced it, and the operation used.
- Calling `.backward()` on an output node topologically sorts the graph and applies the chain rule in reverse to populate `.grad` on every node.
- The autograd engine is roughly 100 lines of Python. A small neural-network library is built on top of it.
- Its purpose is educational: to show that backpropagation is just the repeated application of the chain rule, not something magical.
- Because it operates on individual scalars rather than tensors, it trades runtime efficiency for conceptual clarity.

## Limitations

- The material is a brief, self-described summary of the project; it does not include the source code, a repository location, a license, or version information.
- No dates for the project's creation or releases are given (only the 2026-07-16 ingestion-approval date).
- The line counts are stated approximately ("roughly 100 lines"), and the "small neural-network library" built on top is mentioned but not described in detail.
- No independent corroboration is provided beyond the assertions in the inbox file.
