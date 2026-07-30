# Element: conflict_resolution

**Owning spec:** [DCP Claim](../specs/DCP-CLAIM.md)
**Plane:** Description and negotiation.

## Meaning

`conflict_resolution` states how a claim behaves when another claim overlaps
with its concerns. This is the vocabulary Plane 2 uses to decide whether overlap
is forbidden, negotiable, deferring, or consultative.

## Required

Yes. Every claim MUST include `conflict_resolution`.

## Writers And Readers

Component authors write it. Negotiators, contract generators, and reviewers read
it.

## Fields

- `overlapping_concerns`: declared overlap behavior.
- `ownership_position`: `exclusive`, `negotiable`, `deferring`, or `consulted`.
- `resolution_guidance`: explanation of the scope boundary.
- `precedence_rules`: fallback rules.
- `requires_human_escalation`: whether unresolved conflicts require a human.

## Composition Behavior

Exclusive overlap blocks composition unless scope proves there is no conflict.
Negotiable overlap may produce questions. Deferring overlap yields to a
higher-level owner. Consulted overlap allows participation without ownership.

## Invalid Values

- Missing overlap position.
- Guidance that does not describe scope.
- Conflict rules that contradict domain concerns or refusals.

## Examples

See [aggregate-dcp.yaml](../examples/aggregate-dcp.yaml).
