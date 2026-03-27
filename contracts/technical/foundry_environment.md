# Foundry Environment

This repository is built and maintained with Foundry.

## Tooling baseline

- Solidity: `0.8.29`
- Build/test framework: Foundry (`forge`)
- Output artifacts: `artifacts/`
- Sources: `contracts/`
- Tests: `test/`

## Primary commands

- Build: `forge build`
- Test: `forge test`
- Verbose test: `forge test -vvv`
- Coverage: `forge coverage`
- Format: `forge fmt`
- Docs generation: `forge doc`

## Deployment workflow

- Scripts are in `script/` and run via `forge script ... --broadcast`.
- Protocol addresses should be resolved from `deployments/<chainid>.json`.
- Avoid env-derived protocol-address wiring for previous deployment outputs.

## Documentation workflow

Use:

```sh
./utils/build-contract-docs.sh
```

This command:

1. generates `forge doc` output in a temp directory,
2. transforms it to a solidity-docgen-like structure,
3. syncs references into `.github/wiki/contracts/references`,
4. syncs manual docs into `.github/wiki/contracts/manual`.

## Notes on compatibility

- EventHub routing is expected infrastructure for supported event classes.
- EIP-170 bytecode margin should be checked as part of preflight and CI.
- Keep regression tests in sync with ABI changes (especially subscription and gateway surfaces).
