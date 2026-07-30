# DCP Core Specification

**Status:** Draft.
**Scope:** Product-neutral protocol model.
**Source material:** `docs/HLD-C-dcp-v0.2-internal.md`.

## 1. Purpose

The Domain Claim Protocol (DCP) is a dynamic composite protocol for declaring,
composing, validating, and binding capabilities across multiple levels of a
system. DCP exists so components can be assembled without hidden assumptions or
hard-wired product dependencies.

DCP is not a business payload format, authentication protocol, hosting platform,
or model/tool invocation protocol. It is the contract layer that states whether
a component belongs in a governed assembly, how it composes, and what frozen
contract controls invocation.

## 2. Protocol Planes

DCP has three planes:

| Plane | Question | Output | Runtime cost |
|---|---|---|---|
| Description | What does this component claim about itself? | Claim | Cheap and cacheable |
| Negotiation | Can claims compose under this context and policy? | Composition contract | Expensive but rare |
| Invocation | Can this call happen against the frozen contract? | Deterministic result | Hot path |

Negotiation is a compile step. Invocation is execution. Plane 3 MUST NOT require
fresh model reasoning or design-time negotiation.

## 3. Core Artifacts

DCP defines these artifacts:

- **Claim:** static component self-description, published per component version.
- **Composition contract:** frozen negotiation output binding consumer needs to
  provider offers.
- **Runtime binding:** environment-specific wiring for a frozen contract.
- **Fault vocabulary:** declared runtime faults and propagation behavior.
- **Documentation projection:** generated docs from accepted DCP surfaces.

## 4. Product-Neutrality

DCP MUST remain product-neutral. Concrete products may implement or host DCP, but
the protocol MUST NOT depend on a specific compiler, workflow engine, agent
runtime, AI model, cloud, identity provider, database, or transport.

Product examples are informative unless a spec explicitly marks them normative.

## 5. Transport Boundary

DCP binds semantics before transport. Supported transport choices are named in
claims, offers, contracts, and runtime bindings. The protocol semantics MUST NOT
be defined by a specific transport.

The current transport vocabulary is:

- `in_process`
- `http_json`
- `grpc`

## 6. Design-Time / Runtime Firewall

A runtime call executes against a frozen contract. If runtime evidence violates
a contract assumption, the runtime MUST hard-fail the affected call and emit an
invalidation signal. Runtime MUST NOT silently renegotiate or self-heal by
changing the contract.

## 7. Relationship To MCP

MCP is appropriate when a model invokes a tool at runtime. DCP is appropriate
when a system decides whether capabilities compose before runtime. A component
MAY expose both DCP and MCP surfaces, but DCP governs composition and MCP
governs tool invocation.

## 8. Change Impact

This file establishes the spec-first root for the protocol. It does not change
field names or schema behavior; it reorganizes existing DCP v0.2 concepts into a
public specification page.
