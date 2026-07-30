# Element: refusals

**Owning spec:** [DCP Claim](../specs/DCP-CLAIM.md)
**Plane:** Description and negotiation.

## Meaning

`refusals` state what the component explicitly does not own. Refusals make the
domain boundary reviewable and prevent overbroad claims.

## Required

Yes. `refusals` MUST be non-empty.

## Writers And Readers

Component authors write refusals. Negotiators, reviewers, parent aggregators,
and operators read them.

## Fields

- `concern`: refused concern.
- `rationale`: why the concern is outside this claim.
- `owned_by`: optional but recommended component kind or domain that should own
  the concern instead.

## Composition Behavior

A claimed ownership that conflicts with another claim's refusal may block
composition or require a negotiation question. Parent aggregates project child
refusals when they affect the parent-visible surface.

## Invalid Values

- Empty refusal list.
- Generic refusals such as `everything-else`.
- Refusals without rationale.
- Refusals that contradict declared offers.

## Examples

See [simple-component-claim.yaml](../examples/simple-component-claim.yaml).
