# Element: offers

**Owning spec:** [DCP Claim](../specs/DCP-CLAIM.md)
**Plane:** Description, negotiation, and documentation projection.

## Meaning

`offers` define capabilities this component exposes for other components or
aggregates to consume.

## Required

Yes. The section MUST exist, but the list MAY be empty.

## Writers And Readers

Component authors write offers. Negotiators, contract generators, runtime
binders, documentation projectors, and consumers read them.

## Fields

- `capability`: stable capability name.
- `description`: human-readable explanation.
- `consumer_access`: `any` or `named_components_only`.
- `interface.kind`: `http_api`, `event_stream`, `negotiation`, or `in_process`.
- `interface.details`: deterministic kind-specific map.
- `stability`: `experimental`, `evolving`, `stable`, or `deprecated`.
- `version`: semantic version.
- `cost_implications`: required when the capability uses metered resources.

## Composition Behavior

Plane 2 binds consumer dependencies to provider offers. Generated documentation
MUST expose only accepted offers.

## Invalid Values

- Missing version.
- Interface details that are only prose when deterministic fields are needed.
- Offering a capability that contradicts a refusal.
- Exposing hidden implementation endpoints that are not part of the DCP surface.

## Examples

See [simple-component-claim.yaml](../examples/simple-component-claim.yaml) and
[aggregate-dcp.yaml](../examples/aggregate-dcp.yaml).
