# Chains

The `@elacity-js/core` package defines a single source of truth for **SDK-supported networks** via the `ChainId` enum.

This is used across packages (e.g. `@elacity-js/api` base URL selection, and `@elacity-js/contracts` ecosystem contract address resolution).

## ChainId

```ts
import { ChainId } from '@elacity-js/core';

const chainId = ChainId.Base;
```

### Values

- `ChainId.Base` = `8453`
- `ChainId.ArbitrumSepolia` = `421614`
- `ChainId.Elastos` = `20`

