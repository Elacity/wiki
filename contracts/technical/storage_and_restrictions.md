# Storage and Restrictions

Current notes for storage posture and transfer restriction behavior in V3.

## Storage posture

### Namespaced storage (ERC-7201)

Critical modules use namespaced storage patterns to reduce upgrade-collision risk.

### Linear-slot contracts still tracked

Some upgradeable contracts intentionally still use linear storage layouts and must follow append-only discipline.

## Restriction model

### Operative token semantics

- `ACCESS_TOKEN` and `DISTRIBUTION_RIGHT` are the core restricted-value rights.
- `ROYALTY_SHARE` behavior supports tradability in current architecture.

### Trade permission checks

- Trade flows apply module-level access checks before market actions.
- Channel/operative restriction hooks gate sensitive token movements.

## Verification guidance

When validating deployment/runtime state, focus on:

1. tracker/registry wiring in `CentralStorage`,
2. acknowledgement and role assignment paths for runtime emitters and system contracts,
3. subscription + token-access configuration consistency,
4. payment processor assignment and reward withdraw paths.

## Recommended checks in CI / ops

- run build/test/coverage from Foundry,
- run preflight size/storage checks,
- verify deployment manifests before upgrades,
- validate event routing registration for EventHub emitters.
