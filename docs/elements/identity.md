# Element: identity

**Owning spec:** [DCP Claim](../specs/DCP-CLAIM.md)
**Plane:** Description.

## Meaning

`identity` names the component or aggregate described by a claim. It provides
the stable URI and version other DCP artifacts reference.

## Required

Yes. Every claim MUST include `identity`.

## Writers And Readers

Component authors write `identity`. Catalogs, resolvers, contract generators,
runtime binders, documentation projectors, and operators read it.

## Fields

- `uri`: globally unique stable URI. MUST NOT change across versions.
- `name`: human-readable name.
- `kind`: `intelligent_component`, `component`, or `infrastructure`.
- `version`: semantic version of this component claim.
- `publisher`: publisher name.
- `publisher_uri`: optional publisher URI.

## Composition Behavior

Contracts and runtime bindings reference `identity.uri` and `identity.version`.
Child DCP aggregation also uses these references to preserve inspectability.

## Invalid Values

- Missing `uri`.
- Non-semver `version`.
- A URI reused for a different logical component.
- `kind: intelligent_component` without `negotiation_surface`.

## Examples

See [simple-component-claim.yaml](../examples/simple-component-claim.yaml) and
[aggregate-dcp.yaml](../examples/aggregate-dcp.yaml).
