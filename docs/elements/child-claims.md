# Element: child_claims

**Owning specs:** [DCP Claim](../specs/DCP-CLAIM.md),
[DCP Aggregation](../specs/DCP-AGGREGATION.md)
**Plane:** Description, negotiation, and runtime binding.

## Meaning

`child_claims` references child DCP claims used by an aggregate claim. It is the
standard way to express recursive DCP composition.

## Required

Optional. Required only when a parent claim's capability depends on child DCPs.

## Writers And Readers

Aggregate authors write child references. Projection, negotiation, contract
generation, runtime binding, and review tools read them.

## Fields

- `claim_uri`: child claim URI.
- `role`: role the child plays in the parent aggregate.
- `required`: whether the parent capability requires the child.
- optional policy or scope fields when the parent accepts multiple children.

## Composition Behavior

Projection walks child references deterministically. Parent contracts reference
child contracts, and parent runtime bindings reference child bindings. Child
refusals and faults remain active unless the child claim changes.

## Invalid Values

- Child references that cannot be resolved.
- Cyclic references.
- Hiding child contracts behind product-specific extension blocks.
- Parent offers that depend on children without recording the child reference.

## Examples

See [aggregate-dcp.yaml](../examples/aggregate-dcp.yaml).
