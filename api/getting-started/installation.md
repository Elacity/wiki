# Installation

The `@elacity-js/api` package provides a type-safe client for interacting with Elacity's backend services, including NFT management, authentication, and more.

## Prerequisites

Before installing, ensure you have a project set up with **Node.js (v18+)** and **TypeScript** (if applicable).

The SDK relies on **Ethers.js (v6)** for blockchain interaction and cryptographic signing (SIWE).

## Install the Package

Install the primary API package along with its companion dependency:

```bash
npm install @elacity-js/api ethers
```

Or using yarn:

```bash
yarn add @elacity-js/api ethers
```

## TypeScript Configuration

If you are using TypeScript, ensure your `tsconfig.json` is configured to support modern modules and decorators if you plan to extend the SDK:

```json
{
  "compilerOptions": {
    "target": "ESNext",
    "module": "NodeNext",
    "moduleResolution": "NodeNext",
    "strict": true,
    "esModuleInterop": true
  }
}
```

## Next Steps

Once installed, proceed to the [Authentication](authentication.md) guide to learn how to sign in and interact with protected resources.
