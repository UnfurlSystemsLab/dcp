# Element: child_claims

**Owning specs:** [DCP Claim](../specs/DCP-CLAIM.md),
[DCP Aggregation](../specs/DCP-AGGREGATION.md)
**Plane:** Description, negotiation, and runtime binding.

## Meaning

`child_claims` names the components an aggregate component is built from: its
declared containment (HLD-C2 A.14). Each child is a normal component with its own
claim, artifact, offers, needs and faults. It is the standard way to express
recursive DCP composition for claims.

## Required

Optional. An aggregate component writes it; any other component does not.

## Writers And Readers

Aggregate authors write child references. Projection, negotiation, contract
generation, runtime binding, and review tools read them.

## Fields

- `claim_uri` (required): the child's `identity.uri`, unique within the list.
- `required` (required, no default): `true` when the aggregate cannot run without
  the child.
- `description` (optional): the child's role in the aggregate.

## Composition Behavior

- A composition that selects the aggregate must also select every required child.
  A composer refuses it otherwise, naming the aggregate and the missing child.
- An optional child may be absent. The aggregate runs without it, and neither the
  child nor its artifact is part of the composition.
- Projection draws each selected child under its aggregate, deterministically.
  Parent contracts reference child contracts, and parent runtime bindings
  reference child bindings.
- Child refusals and faults remain active unless the child claim changes. The
  aggregate never restates or re-exports a child's claim.
- `child_claims` is part of the canonical claim hash. A claim without it keeps
  its hash.

## Invalid Values

All of these are `CLAIM_CHILD_CLAIM_INVALID`:
- a missing `claim_uri`;
- the same `claim_uri` twice;
- the claim's own `identity.uri`;
- an entry that does not write `required`;
- children declared both in `child_claims` and in the `metadata.extensions`
  containment bridge.

Also invalid:
- child references that cannot be resolved;
- cyclic references across the claims a composition selects (A names B, B names A), refused by the composer with
  every claim in the cycle named;
- hiding child contracts behind product-specific extension blocks.

## Examples

See [aggregate-dcp.yaml](../examples/aggregate-dcp.yaml).
