# Media Package Installation

> **Package**: `@elacity-js/media`  
> **Version**: 0.0.1

## Installation

The `@elacity-js/media` package depends on other Elacity SDK packages. Install all required dependencies:

```bash
npm install @elacity-js/media @elacity-js/api @elacity-js/contracts @elacity-js/core
# or
yarn add @elacity-js/media @elacity-js/api @elacity-js/contracts @elacity-js/core
```

### Additional Dependencies

For blockchain interactions, you'll also need a contract runner adapter:

**For Ethers.js (v6)**:
```bash
npm install @elacity-js/contracts-ethers-adapter ethers
```

**For Viem**:
```bash
npm install @elacity-js/contracts-viem-adapter viem
```

### Node.js Environment

If using in Node.js (backend), install `form-data` for FormData support:

```bash
npm install form-data
```

## Package Dependencies

- `@elacity-js/api` - API client and background job service
- `@elacity-js/contracts` - Smart contract interactions
- `@elacity-js/core` - Core utilities and types
- `tslib` - TypeScript helper library

## TypeScript Support

The package includes full TypeScript definitions. No additional `@types` packages needed.
