# Domain Claim Protocol

| [Overview](#what-is-dcp) | [Specs](docs/DCP-STANDARDS-SUPPORT.md) | [Schemas](docs/schemas/) | [Elements](docs/elements/) | [Examples](docs/examples/) | [Versions](docs/versions/changelog.md) | [Contributing](CONTRIBUTING.md) | [Security](SECURITY.md) | [Apache 2.0 License](LICENSE) | [Discussions](https://github.com/UnfurlSystemsLab/dcp/discussions) |
|---|---|---|---|---|---|---|---|---|---|
| What DCP is and why it exists | The public specification map | Machine-readable draft shapes | Field and element reference | Minimal and aggregate examples | Changelog and compatibility | Spec change process | Security reporting | Reuse terms | Design questions and proposals |

This repository is the public specification home for the Domain Claim Protocol
(DCP).

## What Is DCP?

The Domain Claim Protocol is a specification for describing software
capabilities before they are assembled.

Modern systems are increasingly composed from independent components, tools,
agents, services, UI modules, infrastructure providers, and runtime adapters.
Those parts may have APIs, but an API alone does not say whether the part should
be used in a specific assembly. DCP fills that gap.

DCP lets a component publish a structured claim that answers:

- What does this component own?
- Where does its responsibility end?
- What does it explicitly refuse to own?
- What dependencies must be present before it can work?
- What capabilities does it offer to other components?
- What conflicts can happen when another component claims the same concern?
- What ports, faults, trust context, and runtime bindings are required?

In short: **DCP turns hidden integration assumptions into explicit, reviewable
contracts.**

## Why It Exists

DCP exists because AI-assisted and component-based systems need a way to reason
about composition without guessing. A model, compiler, runtime, or human
reviewer should not have to infer ownership, safety boundaries, provider needs,
or fault behavior from prose, code, or API shape alone.

DCP gives assembly tools a shared language for deciding:

- whether a component belongs in an assembly;
- which provider can satisfy a consumer need;
- which concerns are owned, refused, shared, or conflicting;
- which child components support an aggregate capability;
- which runtime bindings must exist before execution;
- which generated docs may expose a capability.

## The Three Planes

DCP separates design-time reasoning from runtime execution.

| Plane | Question | Output |
|---|---|---|
| Description | What does this component claim about itself? | Claim |
| Negotiation | Can these claims compose under this context and policy? | Composition contract |
| Invocation | Can this call happen against the frozen contract? | Deterministic runtime result |

Negotiation is a compile step. Invocation is execution. Runtime calls should
execute against a frozen contract, not re-negotiate the system on every call.

## Core Artifacts

- **Claim:** a component's self-description: identity, domain, refusals,
  dependencies, offers, ports, faults, and metadata.
- **Composition contract:** the frozen negotiation result that binds accepted
  needs to accepted offers.
- **Runtime binding:** environment-specific wiring for a frozen contract, using
  secret/config references rather than raw credentials.
- **Referenced child DCPs:** the standard way to build aggregate capabilities
  while keeping child claims, contracts, and bindings inspectable.
- **Documentation projection:** generated Swagger/OpenAPI/AsyncAPI/MCP or human
  docs from accepted DCP capability surfaces only.

## What DCP Is Not

DCP is not a business payload format, auth protocol, deployment platform, model
runtime, or replacement for APIs. It sits above those surfaces. A component may
still expose HTTP, gRPC, events, MCP tools, or in-process calls; DCP describes
whether and how those capabilities compose.

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

## Quick Paths

| Reader Goal | Start With |
|---|---|
| Understand the protocol model | [DCP Core Specification](docs/specs/DCP-CORE.md) |
| Describe a component or adapter | [DCP Claim Specification](docs/specs/DCP-CLAIM.md) |
| Build aggregate/component assemblies | [DCP Aggregation Specification](docs/specs/DCP-AGGREGATION.md) |
| Bind a frozen contract to runtime | [DCP Runtime Binding Specification](docs/specs/DCP-RUNTIME-BINDING.md) |
| Expose generated API/tool docs | [DCP Documentation Projection Specification](docs/specs/DCP-DOCUMENTATION.md) |
| Review current schema fields | [DCP Schema Directory](docs/schemas/) |
| Discuss open design choices | [DCP Discussions](https://github.com/UnfurlSystemsLab/dcp/discussions) |

## Status

DCP is a draft specification. The Java implementation is the executable proof of
the current draft, but the protocol remains open to correction where real
adapter work exposes gaps.

The earlier source documents remain in `docs/HLD-C-*.md` while the public
specification is being split into smaller spec, schema, element, example, and
version documents.

## License

DCP is published under the [Apache License 2.0](LICENSE).

## Participate

- Use Issues for concrete gaps, examples, or schema problems.
- Use Discussions for design questions, tradeoffs, and longer-form proposals.
- Read [Contributing](CONTRIBUTING.md) before changing specs or schemas.
- Read [Security](SECURITY.md) before reporting sensitive protocol or runtime
  binding concerns.
- Bring real component examples whenever possible; DCP is meant to be tested by
  actual adapter and substrate work, not only abstract schema review.
