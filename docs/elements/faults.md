# Element: faults

**Owning specs:** [DCP Claim](../specs/DCP-CLAIM.md),
[DCP Faults](../specs/DCP-FAULTS.md)
**Plane:** Description and invocation.

## Meaning

`faults` declare DCP-visible runtime faults and their propagation behavior.

## Required

Yes. The list MAY be empty if no DCP-visible fault vocabulary is declared.

## Writers And Readers

Component authors write fault vocabulary. Runtimes, operators, parent
aggregates, and documentation projectors read it.

## Fields

- `code`: stable fault code.
- `description`: human-readable explanation.
- `severity`: operational severity.
- `affects`: affected needs, offers, dependencies, constraints, ports, or
  service expectations.
- `evidence`: runtime signals that justify the fault.
- `propagation`: deterministic propagation behavior.
- `remediation`: allowed remediation actions.

## Composition Behavior

Faults propagate to a parent only when they affect a parent-visible surface.
Runtime MUST hard-fail and emit invalidation when a fault violates a frozen
contract assumption.

## Invalid Values

- Untyped log messages presented as DCP faults.
- Propagation rules that require design-time negotiation at runtime.
- Remediation that exceeds the claim boundary without operator approval.

## Examples

See [simple-component-claim.yaml](../examples/simple-component-claim.yaml).
