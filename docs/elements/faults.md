# Element: faults

**Owning specs:** [DCP Claim](../specs/DCP-CLAIM.md),
[DCP Faults](../specs/DCP-FAULTS.md)
**Plane:** Description and invocation.

## Meaning

`faults` declare DCP-visible runtime faults and their propagation behavior.

## Required

Yes, as `faults: { emitted: [...] }`. The `emitted` list MAY be empty if no
DCP-visible fault vocabulary is declared.

## Writers And Readers

Component authors write fault vocabulary. Runtimes, operators, parent
aggregates, and documentation projectors read it.

## Fields

Each `emitted` entry follows [`fault.schema.yaml`](../schemas/fault.schema.yaml)
(HLD-C2 A.10):

- `code`: stable fault code, unique within the claim.
- `category`: the kind of fault, for example `RUNTIME` or `DEPENDENCY`.
- `severity`: `INFO`, `WARNING`, `DEGRADED`, `CRITICAL` or `BLOCKING`.
- `description`: human-readable explanation.
- `affects`: the declared `needs`, `offers` and `constraints` it affects, at
  least one non-empty.
- `evidence.signals`: the runtime signal kinds that justify the fault.
- `propagation`: `parent_impact` (`NONE`, `DEGRADED`, `BLOCKED`),
  `propagates_when` (required unless `NONE`) and an optional `suppresses_when`.
- `remediation.allowed_actions`: the actions the runtime or operator may take.

## Composition Behavior

A fault escalates to its parent aggregate only as its `propagation` declares:
`parent_impact` says how the parent is affected, and `propagates_when` is the
condition evaluated before escalating.
Runtime MUST hard-fail and emit invalidation when a fault violates a frozen
contract assumption.

## Invalid Values

- Untyped log messages presented as DCP faults.
- Propagation rules that require design-time negotiation at runtime.
- Remediation that exceeds the claim boundary without operator approval.

## Examples

See [simple-component-claim.yaml](../examples/simple-component-claim.yaml).
