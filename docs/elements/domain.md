# Element: domain

**Owning spec:** [DCP Claim](../specs/DCP-CLAIM.md)
**Plane:** Description.

## Meaning

`domain` states what the component owns. It is the core boundary statement of a
claim.

## Required

Yes. Every claim MUST include `domain.summary`, at least one `domain.concerns`
entry, and at least one `domain.boundary_principles` entry.

## Writers And Readers

Component authors write `domain`. Negotiators, reviewers, documentation tools,
and parent aggregate projectors read it.

## Fields

- `summary`: one-paragraph statement of the component's domain.
- `concerns`: owned concerns, each with description, optional scope notes,
  owned state, and owned decisions.
- `boundary_principles`: natural-language rules that explain where the domain
  ends.

## Composition Behavior

Domain concerns are compared against refusals, dependencies, offers, and
conflict resolution during negotiation. Parent aggregates project child domains
when deriving the parent-visible surface.

## Invalid Values

- No concerns.
- No boundary principles.
- Concern names that are vague or duplicate within one claim.
- State or decisions described outside the concern that owns them.

## Examples

See [simple-component-claim.yaml](../examples/simple-component-claim.yaml).
