## CoreStorage

This contract serves as the central data repository and registry for the entire ecosystem.
It holds cross-contract data to facilitate state management and data retrieval across all Elacity DRM protocols.
As the single source of truth, CoreStorage inherits multiple specialized trackers and registries:
- **SystemTracker**: Acknowledges and whitelists recognized system contracts.
- **FactoryTracker**: Tracks deployed factory contracts by their operation type.
- **IPTracker**: Registers digital assets and assigns operators for intellectual properties.
- **MarketplaceTracker**: Keeps track of product listings, offers, and platform fee configurations.
- **ChannelRegistry**: Stores records of active distribution channels.
- **ContractRegistry**: A slot-based registry for vital ecosystem contract addresses.
- **IPMapperModule**: Maps external definitions and resources to ecosystem intellectual properties.
- **FeesInformation**: Holds administrative configurations and global protocol fee structures.

_This contract is designed to be fully upgradeable and aggregates state management._

