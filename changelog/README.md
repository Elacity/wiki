---
description: Release notes for the js-sdk monorepo
icon: clock-rotate-left
---

# Changelog

This changelog tracks releases for the `<js-sdk>` repository.

## v0.3

### 0.3.1 - 2026-04-01

Reference range: `f58b926..HEAD`

#### Changed

* add pre-release test gate for common and adapter packages (`a6ded65`)
* fix test (`8f007e5`)
* add preflight check before release (`549261e`)
* enforce release on new tag (`51e6d32`)

#### Added

* added changelog generator for the documentation (`04b2090`)

#### Docs

* open-api integration (cleanup and doc integration) (`d0bbc95`)

#### Commits

* `51e6d32` 2026-04-01 ci: enforce release on new tag
* `549261e` 2026-04-01 ci: add preflight check before release
* `8f007e5` 2026-04-01 ci: fix test
* `a6ded65` 2026-03-31 ci: add pre-release test gate for common and adapter packages
* `04b2090` 2026-03-31 feat: added changelog generator for the documentation
* `d0bbc95` 2026-03-31 docs: open-api integration (cleanup and doc integration)

#### Scope

* `.github/workflows` (CI/CD updates)
* `scripts` (changelog automation)
* `packages/common` and adapter packages (pre-release test coverage)
* `packages/api` (OpenAPI documentation integration)

#### Package Versions

* `<js-sdk>`: `0.2.0-beta.18`
* `@elacity-js/api`: `0.9.0`
* `@elacity-js/common`: `1.1.0`
* `@elacity-js/contracts`: `3.1.1`
* `@elacity-js/contracts-ethers-adapter`: `0.2.0`
* `@elacity-js/contracts-ua-executor`: `0.2.0`
* `@elacity-js/contracts-viem-adapter`: `0.2.0`
* `@elacity-js/media-packager`: `0.2.0`

### 0.3.0 - 2026-03-31

Reference range: `1c396b9..f58b926`

#### Added

* enabled protocol 3.0 support (`a0ac250`)

#### Changed

* more adjustments (`d2486ad`)

#### Package Versions

* `<js-sdk>`: `0.2.0-beta.18`
* `@elacity-js/api`: **`0.9.0`**
* `@elacity-js/common`: **`1.1.0`**
* `@elacity-js/contracts`: **`3.1.1`**
* `@elacity-js/contracts-ethers-adapter`: **`0.2.0`**
* `@elacity-js/contracts-ua-executor`: **`0.2.0`**
* `@elacity-js/contracts-viem-adapter`: **`0.2.0`**
* `@elacity-js/media-packager`: **`0.2.0`**

## v0.2

### 0.2.0-beta - Initial baseline

Reference commit: `1c396b9`

* Baseline state for the current js-sdk release line.
* Subsequent changes for `0.3.1` are tracked from this commit forward.

#### Package Versions

* `<js-sdk>`: `0.2.0-beta.18`
* `@elacity-js/api`: `0.8.5-beta.24`
* `@elacity-js/common`: `1.0.0-beta.23`
* `@elacity-js/contracts`: `0.8.2-beta.23`
* `@elacity-js/contracts-ethers-adapter`: `0.0.1-beta.23`
* `@elacity-js/contracts-ua-executor`: `0.0.1-beta.23`
* `@elacity-js/contracts-viem-adapter`: `0.0.1-beta.23`
* `@elacity-js/media-packager`: `0.1.0-beta.23`
