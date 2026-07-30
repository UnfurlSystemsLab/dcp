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

1. [DCP Standards Support](docs/DCP-STANDARDS-SUPPORT.md)
2. [DCP Core Specification](docs/specs/DCP-CORE.md)
3. [DCP Claim Specification](docs/specs/DCP-CLAIM.md)
4. [DCP Schema Directory](docs/schemas/)
5. [DCP Element Directory](docs/elements/)
6. [Java implementation repository](https://github.com/UnfurlSystemsLab/unfurl-dcp)

## Status

DCP is a draft specification. The Java implementation is the executable proof of
the current draft, but the protocol remains open to correction where real
adapter work exposes gaps.

The earlier source documents remain in `docs/HLD-C-*.md` while the public
specification is being split into smaller spec, schema, element, example, and
version documents.

## Participate

- Use Issues for concrete gaps, examples, or schema problems.
- Use Discussions for design questions, tradeoffs, and longer-form proposals.
- Bring real component examples whenever possible; DCP is meant to be tested by
  actual adapter and substrate work, not only abstract schema review.
