# Element: integration_ports

**Owning specs:** [DCP Claim](../specs/DCP-CLAIM.md),
[DCP Runtime Binding](../specs/DCP-RUNTIME-BINDING.md)
**Plane:** Description and deployment/runtime projection.

## Meaning

`integration_ports` name common enterprise integration concerns so they can be
governed through DCP instead of hidden in product-specific configuration.

## Required

Optional, but strongly recommended for production components.

## Writers And Readers

Component authors write claim-level port requirements. Deployment authors and
runtime binders write concrete binding references. Resolvers, operators, and
documentation projectors read them.

## Ports

The claim's typed integration surfaces (HLD-C2 A.9), each optional; an absent one is not declared, never a default:

- `authentication`: whether a caller must authenticate, and the accepted modes
- `authorization`: whether authorization is checked, who decides, and the permission prefixes
- `telemetry`: the signals the component really emits, and whether it needs a correlation id
- `monitoring`: the health endpoints it serves (absolute paths, never URLs), and its readiness and liveness checks
- `ai`: whether it needs AI capability to operate, and how it reaches a model

Beside them, `ports` is the component's own named wiring (for example a host's runtime ports, name to implementation
type). DCP carries it unchanged and never interprets it.

Secrets and configuration are not claim ports: a deployment supplies them in the runtime binding's `configuration`
(`config_refs`, `secret_refs`), as references only.

## Composition Behavior

Ports may create dependencies that must be satisfied before contract generation
or runtime activation. Runtime binding resolves ports to environment-specific
references without putting raw credentials in the DCP artifact.

## Invalid Values

- Raw credentials.
- Product-specific local configuration that has no DCP concept.
- Ports that bypass accepted contract capabilities.

## Examples

See [runtime-binding.yaml](../examples/runtime-binding.yaml).
