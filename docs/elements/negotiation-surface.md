# Element: negotiation_surface

**Owning specs:** [DCP Claim](../specs/DCP-CLAIM.md),
[DCP Negotiation](../specs/DCP-NEGOTIATION.md)
**Plane:** Description and negotiation.

## Meaning

`negotiation_surface` declares how an intelligent component can answer
design-time negotiation questions about its own claim boundary.

## Required

Required when `identity.kind == intelligent_component`. Optional otherwise.

## Writers And Readers

Intelligent component authors write it. Authoring agents, human review tools,
and negotiation services read it.

## Fields

- `endpoint`: negotiation endpoint.
- `protocols_supported`: supported request protocol names.
- `supported_intents`: bounded intents the component can answer.
- `answer_grounding`: where answers come from.
- `limitations`: what the surface refuses to answer.

## Composition Behavior

Negotiation surfaces MAY help resolve Plane 2 questions. They MUST NOT be called
in Plane 3 invocation to renegotiate runtime behavior.

## Invalid Values

- Missing on an intelligent component.
- Limitations that permit answering outside the claim boundary.
- Grounding that hides whether answers use live state, retrieval, or training.

## Examples

See [aggregate-dcp.yaml](../examples/aggregate-dcp.yaml).
