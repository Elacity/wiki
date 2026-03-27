# Contract References

This directory contains auto-generated documentation for all smart contracts in the Elacity DRM ecosystem.

## Contents

- **[AuthorityGateway](AuthorityGateway.md)** - Access control and licensing gateway
- **[TradeGateway](TradeGateway.md)** - General asset trading gateway
- **[Ecosystem](Ecosystem.md)** - Ecosystem contract overview

### Component Directories

- **[assets/](assets/)** - Asset-related contracts
- **[channel/](channel/)** - Channel contracts (StandardChannel, MultiChannel)
- **[library/](library/)** - Shared library contracts and utilities
- **[modules/](modules/)** - Modular components (License, Trade, Payment, Royalty, Subscription, etc.)
- **[operative/](operative/)** - Operative contracts (OperativeBuyable, OperativeBuyableSellable)
- **[proxy/](proxy/)** - Proxy pattern contracts
- **[storage/](storage/)** - CoreStorage and tracker components


## Documentation Structure

Each contract file includes:
- Contract description
- Constructor parameters
- Public/external functions with parameters and return values
- Events and their parameters
- Modifiers and access control
- Inheritance hierarchy

---

> **Note**: This documentation is auto-generated and should not be manually edited. Any changes will be overwritten on the next build. To update documentation, modify the NatSpec comments in the Solidity source files.
