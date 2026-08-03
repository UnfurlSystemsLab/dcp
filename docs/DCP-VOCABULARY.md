# DCP Vocabulary

**Status:** Draft informative vocabulary.
**Audience:** Component authors, adapter authors, specification reviewers,
runtime implementers, and product teams using DCP.

This page defines the shared language used across the Domain Claim Protocol
(DCP) specification set. It is a reader aid, not a replacement for the
normative specification pages.

If this vocabulary conflicts with an owning spec, schema, or element page, the
owning document controls.

## Reading Rules

- Protocol terms are product-neutral unless explicitly marked informative.
- Field names are shown in `snake_case`.
- A "DCP artifact" means a claim, composition contract, runtime binding,
  documentation projection, or another protocol-defined document.
- "MUST", "MUST NOT", "SHOULD", "SHOULD NOT", and "MAY" are normative only
  when they appear in an owning specification page.

## Core Protocol Terms

| Term | Meaning | Primary Reference |
|---|---|---|
| DCP | Domain Claim Protocol: a product-neutral protocol for declaring, composing, validating, and binding capabilities before runtime execution. | [Core](specs/DCP-CORE.md) |
| Plane | One of the major DCP phases: description, negotiation, or invocation. | [Core](specs/DCP-CORE.md) |
| Description plane | The phase where a component publishes a static claim describing what it owns, refuses, needs, and offers. | [Core](specs/DCP-CORE.md), [Claim](specs/DCP-CLAIM.md) |
| Negotiation plane | The design-time phase where claims, context, and policy are evaluated to produce a frozen composition contract. | [Core](specs/DCP-CORE.md), [Negotiation](specs/DCP-NEGOTIATION.md) |
| Invocation plane | The hot-path runtime phase where calls execute against a frozen contract without repeating design-time negotiation. | [Core](specs/DCP-CORE.md) |
| Design-time / runtime firewall | The rule that negotiation happens before runtime, and runtime invocation must use the frozen contract instead of renegotiating. | [Core](specs/DCP-CORE.md) |
| Product-neutrality | The rule that DCP semantics must not depend on a specific product, cloud, model provider, identity system, workflow engine, database, or transport. | [Core](specs/DCP-CORE.md) |
| Transport boundary | The distinction between DCP's semantic contract and the concrete transport used to invoke a provider. | [Core](specs/DCP-CORE.md) |
| MCP | Model Context Protocol. DCP may compose components that expose MCP tools, but MCP governs tool invocation while DCP governs composition. | [Core](specs/DCP-CORE.md) |

## Artifact Terms

| Term | Meaning | Primary Reference |
|---|---|---|
| Claim | A static, versioned component self-description. It names identity, domain, refusals, dependencies, offers, conflicts, ports, faults, and metadata. | [Claim](specs/DCP-CLAIM.md), [Claim schema](schemas/claim.schema.yaml) |
| Composition contract | The frozen negotiation output that records accepted bindings, rejected needs, participating claims, policy, trust context, and child contracts when applicable. | [Contract](specs/DCP-CONTRACT.md), [Contract schema](schemas/contract.schema.yaml) |
| Runtime binding | Environment-specific wiring for a frozen contract. It resolves providers, transports, ports, and secret/config references without introducing new capabilities. | [Runtime Binding](specs/DCP-RUNTIME-BINDING.md), [Runtime binding schema](schemas/runtime-binding.schema.yaml) |
| Webapp manifest | A frontend or host-shell projection of a DCP claim. It exposes UI-visible capabilities from the same claim boundary. | [Webapp Manifest](specs/DCP-WEBAPP-MANIFEST.md), [Webapp manifest schema](schemas/webapp-manifest.schema.yaml) |
| Fault vocabulary | Declared DCP-visible fault codes, evidence, affected surfaces, propagation rules, and remediation options. | [Faults](specs/DCP-FAULTS.md), [Fault schema](schemas/fault.schema.yaml) |
| Documentation projection | Generated OpenAPI, AsyncAPI, MCP/tool, or human-readable documentation derived only from accepted DCP surfaces. | [Documentation](specs/DCP-DOCUMENTATION.md), [Documentation projection schema](schemas/documentation-projection.schema.yaml) |
| Action context | Structured design-time context that describes the authoring or editing action currently being negotiated. | [Negotiation](specs/DCP-NEGOTIATION.md), [Action context schema](schemas/action-context.schema.yaml) |
| Negotiation question | A structured question raised during negotiation when a choice, missing input, or policy decision blocks or affects contract generation. | [Negotiation](specs/DCP-NEGOTIATION.md), [Negotiation question schema](schemas/negotiation-question.schema.yaml) |

## Claim Element Terms

| Term | Meaning | Primary Reference |
|---|---|---|
| `identity` | The stable identity of the component or aggregate, including URI, name, kind, version, and publisher. | [identity](elements/identity.md) |
| Claim URI | The stable URI in `identity.uri` used by contracts, runtime bindings, child references, catalogs, and documentation projections. | [identity](elements/identity.md) |
| `domain` | The bounded area of responsibility the claim owns, including summary, concerns, ownership statements, and boundary principles. | [domain](elements/domain.md) |
| Concern | A named responsibility or problem area owned, refused, shared, or constrained by a claim. | [domain](elements/domain.md), [refusals](elements/refusals.md) |
| Boundary principle | A rule that explains where the component's responsibility starts or ends. | [domain](elements/domain.md) |
| `refusals` | Explicit statements of what the component does not own and which kind of component should own that concern instead. | [refusals](elements/refusals.md) |
| `dependencies` | Needs the component requires, recommends, or forbids before it can compose or run correctly. | [dependencies](elements/dependencies.md) |
| Need | A consumer-side requirement declared in `dependencies` and later bound, rejected, or left unresolved by negotiation. | [dependencies](elements/dependencies.md), [Contract](specs/DCP-CONTRACT.md) |
| `offers` | Provider-side capabilities the component exposes for other components to consume. | [offers](elements/offers.md) |
| Capability | A stable named ability offered or required by a component, such as `document.search` or `model.generate`. | [offers](elements/offers.md), [dependencies](elements/dependencies.md) |
| Capability surface | The set of accepted capabilities visible through a claim, contract, runtime binding, or documentation projection. | [Documentation](specs/DCP-DOCUMENTATION.md) |
| Consumer | A claim or component that needs a capability from another claim. | [Contract](specs/DCP-CONTRACT.md) |
| Provider | A claim or component that offers a capability to a consumer. | [offers](elements/offers.md), [Contract](specs/DCP-CONTRACT.md) |
| `integration_ports` | DCP-visible requirements for cross-cutting integration concerns such as auth, authorization, telemetry, monitoring, secrets, configuration, and model access. | [integration_ports](elements/integration-ports.md) |
| Port | A named integration concern that must be satisfied by runtime binding or environment wiring before execution. | [integration_ports](elements/integration-ports.md) |
| `conflict_resolution` | Claim-level rules for handling ownership conflicts, incompatible providers, or mutually exclusive concerns. | [conflict_resolution](elements/conflict-resolution.md) |
| `negotiation_surface` | The claim-declared surface an intelligent component can use to answer design-time questions about its own boundary. | [negotiation_surface](elements/negotiation-surface.md) |
| `faults` | Declared protocol-visible faults and their propagation behavior. | [faults](elements/faults.md) |
| `child_claims` | References from a parent claim to child claims used for aggregate composition. | [child_claims](elements/child-claims.md), [Aggregation](specs/DCP-AGGREGATION.md) |
| `metadata` | Version, provenance, source, signature, documentation, and review metadata attached to DCP artifacts. | [metadata](elements/metadata.md) |

## Composition And Aggregation Terms

| Term | Meaning | Primary Reference |
|---|---|---|
| Assembly | An evaluated set of claims intended to satisfy a client or tenant need. Assembly is an implementation activity that should produce DCP contracts and bindings when accepted. | [Core](specs/DCP-CORE.md), [Contract](specs/DCP-CONTRACT.md) |
| Aggregation | Parent/child DCP composition where a parent claim, contract, or runtime binding references child artifacts. | [Aggregation](specs/DCP-AGGREGATION.md) |
| Aggregate DCP | A parent DCP artifact that exposes a higher-level capability supported by parent-owned capabilities, child capabilities, or both. | [Aggregation](specs/DCP-AGGREGATION.md), [Aggregate example](examples/aggregate-dcp.yaml) |
| Child claim | A referenced claim that participates in a parent claim's aggregate capability. | [child_claims](elements/child-claims.md) |
| Child contract | A referenced contract that preserves the accepted bindings behind a parent contract's aggregate capability. | [Contract](specs/DCP-CONTRACT.md), [Aggregation](specs/DCP-AGGREGATION.md) |
| Child binding | A referenced runtime binding that preserves environment wiring behind a parent runtime binding. | [Runtime Binding](specs/DCP-RUNTIME-BINDING.md), [Aggregation](specs/DCP-AGGREGATION.md) |
| Projection | A deterministic view derived by walking DCP references, accepted bindings, and policy. | [Aggregation](specs/DCP-AGGREGATION.md), [Documentation](specs/DCP-DOCUMENTATION.md) |
| Accepted binding | A contract record that binds a consumer need to a provider offer. | [Contract](specs/DCP-CONTRACT.md) |
| Rejected need | A need that negotiation could not bind, with a recorded reason. | [Contract](specs/DCP-CONTRACT.md) |
| Unresolved need | A need that remains open because required information, policy, or provider evidence is missing. | [Negotiation](specs/DCP-NEGOTIATION.md), [Contract](specs/DCP-CONTRACT.md) |
| Recursive closure | The full set of parent and child claims, contracts, and bindings needed to prove an aggregate capability. | [Aggregation](specs/DCP-AGGREGATION.md) |

## Runtime And Documentation Terms

| Term | Meaning | Primary Reference |
|---|---|---|
| Secret reference | A pointer to a secret managed outside the DCP artifact. Raw credentials must not be embedded in runtime bindings. | [Runtime Binding](specs/DCP-RUNTIME-BINDING.md) |
| Config reference | A pointer to environment-specific configuration managed outside the DCP artifact. | [Runtime Binding](specs/DCP-RUNTIME-BINDING.md) |
| Runtime provider | The environment-specific provider endpoint or implementation selected by the runtime binding for an accepted contract provider. | [Runtime Binding](specs/DCP-RUNTIME-BINDING.md) |
| Runtime verification | Checks that the contract, runtime binding, provider references, ports, secrets, and config references are present and consistent before execution. | [Runtime Binding](specs/DCP-RUNTIME-BINDING.md) |
| Invalidation signal | A deterministic runtime signal emitted when evidence violates a frozen contract assumption. | [Core](specs/DCP-CORE.md), [Faults](specs/DCP-FAULTS.md) |
| Propagation gate | The deterministic rule that decides whether a child fault affects a parent-visible need, dependency, constraint, offer, or service expectation. | [Faults](specs/DCP-FAULTS.md) |
| Swagger/OpenAPI projection | A documentation projection for accepted HTTP capability surfaces only. | [Documentation](specs/DCP-DOCUMENTATION.md) |
| Tool documentation projection | A documentation projection for accepted MCP/tool capability surfaces only. | [Documentation](specs/DCP-DOCUMENTATION.md) |

## Related Or Informative Terms

These terms appear in discussions, examples, or product integrations. They are
included here to reduce ambiguity, but they are not standalone protocol
artifacts unless an owning DCP spec defines them.

| Term | Meaning | Primary Reference |
|---|---|---|
| Adapter | A component that maps an external product, service, library, or runtime into DCP claims, contracts, bindings, or documentation projections. | [Claim](specs/DCP-CLAIM.md) |
| Catalog | A product or tool-maintained collection of DCP claims and related artifacts available for negotiation. | [Core](specs/DCP-CORE.md) |
| Component | Any independently described unit that can publish a DCP claim. A component may be a service, tool, agent, UI module, infrastructure provider, runtime adapter, or aggregate. | [Claim](specs/DCP-CLAIM.md) |
| Substrate | Informative term for a reusable capability layer or product integration that may publish DCP claims. It is not a required DCP artifact type by itself. | [Claim](specs/DCP-CLAIM.md) |
| Agent | Informative term for an intelligent component or runtime participant. DCP can describe and compose agents, but does not mandate an agent runtime. | [Core](specs/DCP-CORE.md), [Claim](specs/DCP-CLAIM.md) |
| Tool | Informative term for an executable capability, often exposed through MCP or another runtime protocol. DCP can decide whether the tool capability composes before runtime. | [Core](specs/DCP-CORE.md), [Documentation](specs/DCP-DOCUMENTATION.md) |
| Tenant | Informative term for the deployment or customer scope used by products and runtime bindings. DCP artifacts may be filtered or bound per tenant. | [Runtime Binding](specs/DCP-RUNTIME-BINDING.md), [Documentation](specs/DCP-DOCUMENTATION.md) |

## Change Impact

This page adds a shared vocabulary for the current DCP v0.2 draft. It does not
introduce new schema fields or protocol semantics.
