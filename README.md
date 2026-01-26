# Elacity JS SDK Documentation

Welcome to the official documentation for Elacity's JavaScript SDKs. This suite of tools enables you to integrate Elacity's backend services, manage NFTs, interact with smart contracts, and build rich media experiences.

## SDK Packages

The Elacity SDK suite is designed to be modular, type-safe, and easy to integrate into any modern JavaScript or TypeScript environment. Each package serves a specific purpose:

### 📦 [@elacity-js/core](core/SUMMARY.md)

Core utilities and shared interfaces used across all SDK packages:
- **Authentication**: Shared `AuthUser` and `AuthSigner` interfaces for SIWE-based authentication
- **Pagination**: Global types for handling large data sets efficiently
- **Storage**: Shared interfaces for session persistence

[→ View Core documentation](core/SUMMARY.md)

---

### 🌐 [@elacity-js/api](api/SUMMARY.md)

REST and GraphQL API client for interacting with Elacity's backend services:
- Multi-network support (Elastos, Base, Arbitrum Sepolia)
- SIWE (Sign-In with Ethereum) authentication
- Services for NFTs, Collections, Channels, and Identity management

[→ View API documentation](api/SUMMARY.md)

---

### ⚡ [@elacity-js/contracts](contracts/SUMMARY.md)

Framework-agnostic smart contract wrappers for interacting with Elacity's on-chain contracts:
- Support for popular libraries (Ethers.js, Viem, Web3.js)
- Type-safe contract interactions
- Factory, vault, and token contract wrappers

[→ View Contracts documentation](contracts/SUMMARY.md)

---

## Getting Started

Choose the package that best fits your needs:

- **Building a web app with backend integration?** Start with the [API package](api/SUMMARY.md)
- **Interacting directly with smart contracts?** Check out the [Contracts package](contracts/SUMMARY.md)
- **Need shared utilities?** Explore the [Core package](core/SUMMARY.md)

## Support

For issues, questions, or contributions, please visit our GitHub repository or contact the Elacity team.

