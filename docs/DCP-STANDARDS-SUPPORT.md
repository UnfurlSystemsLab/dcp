# DCP Standards Support

**Status:** Draft public specification index.
**Audience:** Component authors, adapter authors, runtime implementers, product
architects, and reviewers.

This page is the entry point for the Domain Claim Protocol (DCP) specification
set. It is intentionally an index before it is a schema reference: readers
should understand the protocol surfaces before reading field-level definitions.

## Orientation

| Document | Purpose |
|---|---|
| [DCP Vocabulary](DCP-VOCABULARY.md) | Defines common DCP terms and points each term back to its owning spec or element page. |

## Specification Set

| Document | Purpose | Status |
|---|---|---|
| [DCP Core](specs/DCP-CORE.md) | Defines the protocol model, planes, lifecycle, and product-neutral boundaries. | Draft |
| [DCP Claim](specs/DCP-CLAIM.md) | Defines component self-description: identity, domain, refusals, dependencies, offers, ports, faults, and metadata. | Draft |
| [DCP Composition Contract](specs/DCP-CONTRACT.md) | Defines the frozen negotiation output that binds needs to offers. | Draft |
| [DCP Runtime Binding](specs/DCP-RUNTIME-BINDING.md) | Defines environment-specific deployment/runtime wiring for a frozen contract. | Draft |
| [DCP Webapp Manifest](specs/DCP-WEBAPP-MANIFEST.md) | Defines the frontend/host-shell projection of a claim. | Draft |
| [DCP Faults](specs/DCP-FAULTS.md) | Defines declared fault vocabulary, deterministic propagation, and invalidation behavior. | Draft |
| [DCP Aggregation](specs/DCP-AGGREGATION.md) | Defines parent/child DCP references and recursive projection. | Draft |
| [DCP Negotiation](specs/DCP-NEGOTIATION.md) | Defines question schemas and action-scoped design-time context. | Draft |
| [DCP Documentation](specs/DCP-DOCUMENTATION.md) | Defines generated capability documentation from accepted DCP surfaces. | Draft |

## Schema Set

| Schema | Artifact |
|---|---|
| [Claim schema](schemas/claim.schema.yaml) | Plane 1 component self-description |
| [Composition contract schema](schemas/contract.schema.yaml) | Plane 2 frozen composition artifact |
| [Runtime binding schema](schemas/runtime-binding.schema.yaml) | Deployment/runtime wiring projection |
| [Webapp manifest schema](schemas/webapp-manifest.schema.yaml) | Frontend/host-shell projection of a claim |
| [Fault schema](schemas/fault.schema.yaml) | Declared fault vocabulary and propagation policy |
| [Negotiation question schema](schemas/negotiation-question.schema.yaml) | Plane 2 question structure |
| [Action context schema](schemas/action-context.schema.yaml) | Design-time authoring/edit intent |
| [Documentation projection schema](schemas/documentation-projection.schema.yaml) | Generated capability documentation from accepted DCP state |

Schemas are normative only when their owning specification says so. If a schema
field is unclear, the owning spec and element page control the interpretation.

## Element Reference

| Element | Owning spec |
|---|---|
| [identity](elements/identity.md) | DCP Claim |
| [domain](elements/domain.md) | DCP Claim |
| [refusals](elements/refusals.md) | DCP Claim |
| [dependencies](elements/dependencies.md) | DCP Claim |
| [offers](elements/offers.md) | DCP Claim |
| [integration_ports](elements/integration-ports.md) | DCP Claim and Runtime Binding |
| [conflict_resolution](elements/conflict-resolution.md) | DCP Claim |
| [negotiation_surface](elements/negotiation-surface.md) | DCP Claim and DCP Negotiation |
| [faults](elements/faults.md) | DCP Claim and DCP Faults |
| [child_claims](elements/child-claims.md) | DCP Claim and DCP Aggregation |
| [metadata](elements/metadata.md) | All DCP artifacts |

## Examples

| Example | Use |
|---|---|
| [Minimal component claim](examples/simple-component-claim.yaml) | Smallest readable claim shape |
| [Aggregate DCP](examples/aggregate-dcp.yaml) | Parent claim with referenced child DCPs |
| [Runtime binding](examples/runtime-binding.yaml) | Binding a contract to concrete provider endpoints and configuration references |

## Versions And Compatibility

- [Changelog](versions/changelog.md)
- [Compatibility](versions/compatibility.md)

## Relationship To Implementations

This repository defines the protocol. The Java implementation lives in
[`UnfurlSystemsLab/unfurl-dcp`](https://github.com/UnfurlSystemsLab/unfurl-dcp).
Implementation notes may explain how a library enforces the spec, but they must
not define DCP behavior before the protocol documents define it.

## Source Material During Transition

The repository was seeded from two larger internal documents:

- [DCP v0.2 internal specification](HLD-C-dcp-v0.2-internal.md)
- [DCP schema specification](HLD-C2-dcp-schema-spec-updated.md)

Those files remain as source material while the public spec set is split into
the documents above. If there is a conflict during the transition, prefer the
new spec-first file and record the conflict in the changelog or an open
question.
