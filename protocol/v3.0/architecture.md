# Architecture Notes

## High-level domains

The v3 contract graph is grouped into:

- `channel/`: channel factories, registries, and channel kinds.
- `operative/`: operative primitives and asset lifecycle contracts.
- `modules/`: shared protocol modules (payment, trade, royalty, subscription, access control, core storage adapters).
- `storage/`: central trackers and protocol state contracts.
- `events/`: event hub and resolver surfaces.

## Entry gateways

At the top level, ecosystem-facing gateways and coordinators include:

- `AuthorityGateway`
- `RoyaltyTradeGateway`
- `Ecosystem`

Use the reference docs for exact signatures and inheritance maps.
