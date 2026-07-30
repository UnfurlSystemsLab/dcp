# Domain Claim Protocol

This repository is the public specification home for the Domain Claim Protocol
(DCP).

DCP is the contract language Unfurl uses when software components need to
describe what they own, what they refuse, what they need, what they offer, what
can fail, and how they may be bound into a governed assembly.

## Repository Role

Use this repository for:

- Protocol specification and schema discussion.
- Draft examples and model review.
- GitHub Issues and Discussions about the DCP shape.
- Public-facing coordination for adapter and component authors.

Do not use this repository for the Java implementation. The Java library lives
in [`UnfurlSystemsLab/unfurl-dcp`](https://github.com/UnfurlSystemsLab/unfurl-dcp).

## Start Here

1. [DCP v0.2 Internal Specification](docs/HLD-C-dcp-v0.2-internal.md)
2. [DCP Schema Specification](docs/HLD-C2-dcp-schema-spec-updated.md)
3. [Java implementation repository](https://github.com/UnfurlSystemsLab/unfurl-dcp)

## Status

DCP is a draft specification. The Java implementation is the executable proof of
the current draft, but the protocol remains open to correction where real
adapter work exposes gaps.

## Participate

- Use Issues for concrete gaps, examples, or schema problems.
- Use Discussions for design questions, tradeoffs, and longer-form proposals.
- Bring real component examples whenever possible; DCP is meant to be tested by
  actual adapter and substrate work, not only abstract schema review.
