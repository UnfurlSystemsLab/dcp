# Element: dependencies

**Owning spec:** [DCP Claim](../specs/DCP-CLAIM.md)
**Plane:** Description and negotiation.

## Meaning

`dependencies` state what must, should, or must not be present for the component
to compose safely.

## Required

Yes. The section MUST exist. Its `required`, `recommended`, and `forbidden`
lists MAY be empty.

## Writers And Readers

Component authors write dependencies. Catalog admission, negotiation, contract
generation, runtime binding, and operators read them.

## Fields

- `required`: needs that block composition when unsatisfied.
- `recommended`: needs that improve operation but do not block composition.
- `forbidden`: capabilities or ownership overlaps that block composition when
  present.

## Composition Behavior

Plane 2 binds required needs to accepted provider offers. Forbidden dependencies
express predeclared exclusive conflicts.

## Invalid Values

- A required dependency without accepted provider criteria.
- A forbidden dependency without a reason.
- A dependency that secretly names a concrete deployment credential rather than
  a capability or provider shape.

## Examples

See [simple-component-claim.yaml](../examples/simple-component-claim.yaml) and
[runtime-binding.yaml](../examples/runtime-binding.yaml).
