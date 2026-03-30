# Generalities

## What changed from v2

The v3 protocol updates the contract surface and architecture:

- Contracts are grouped by clear domains (modules, channel, operative, storage, events).
- Several legacy flows were removed or reworked to match the current protocol model.
- Gateway and module responsibilities are more explicit for integrators.

## Supported chains

Current targets used by this repository:

- Arbitrum Sepolia (`421614`)
- Base (`84531`)
- Elastos Smart Chain (`20`)

## Source of truth

- Solidity sources: `contracts/`
- Tests: `test/`
- Deploy scripts: `script/`
- Deploy manifests: `deployments/<chainid>.json`

## Versioning posture

- v2 docs are kept for backward compatibility.
- v3 is the active protocol line for ongoing implementation.
