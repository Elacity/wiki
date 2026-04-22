# CentralStorage
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/52ca0e7824ef5fab5ebe0a131f7c6e6dd330de09/contracts/storage/CentralStorage.sol)

**Inherits:**
Initializable, [IStorage](/contracts/storage/IStorage.md), [SystemTracker](/contracts/storage/SystemTracker.md), [IPTracker](/contracts/storage/IPTracker.md), [MarketplaceTracker](/contracts/storage/MarketplaceTracker.md), [ChannelRegistry](/contracts/channel/ChannelRegistry.md), [FeesInformation](/contracts/storage/FeesInformation.md)

**Title:**
CentralStorage - The Central Intelligence & Data Hub of the Elacity DRM Ecosystem

This contract serves as the central data repository and registry for the entire ecosystem.
It holds cross-contract data to facilitate state management and data retrieval across all Elacity DRM protocols.
As the single source of truth, CentralStorage inherits multiple specialized trackers and registries:
- **SystemTracker**: Acknowledges recognized system contracts and stores slot-based protocol contract addresses.
- **IPTracker**: Registers digital assets and assigns operators for intellectual properties and maps IP to a channel token reference.
- **MarketplaceTracker**: Keeps track of product listings, offers, and platform fee configurations.
- **ChannelRegistry**: Stores records of active distribution channels.
- **FeesInformation**: Holds administrative configurations and global protocol fee structures.

This contract is designed to be fully upgradeable and aggregates state management.


## Functions
### constructor

**Notes:**
- oz-upgrades-unsafe-allow: constructor

- docs-ignore: true


```solidity
constructor() ;
```

### initialize

**Note:**
docs-ignore: true


```solidity
function initialize(address initialOwner) public initializer;
```

### owner

Required explicit override because `CentralStorage` inherits both
`OwnableUpgradeable` (which implements `owner()`) and `MarketplaceTracker`
(which declares `owner()` as virtual for access-control checks). This
function only resolves the inheritance graph and preserves standard
Ownable behavior.


```solidity
function owner() public view override(OwnableUpgradeable, MarketplaceTracker) returns (address);
```

