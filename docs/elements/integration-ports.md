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

## Common Ports

- `authentication`
- `authorization`
- `telemetry`
- `monitoring`
- `secrets`
- `configuration`
- `ai`

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
