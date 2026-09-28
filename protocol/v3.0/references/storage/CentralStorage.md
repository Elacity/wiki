# CentralStorage
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/bc1f2ea3fd5d8b703627a7946e7d5fe7fb13f047/contracts/storage/CentralStorage.sol)

**Inherits:**
Initializable, [IStorage](/contracts/storage/IStorage.md), [SystemTracker](/contracts/storage/SystemTracker.md), [IPTracker](/contracts/storage/IPTracker.md), [MarketplaceTracker](/contracts/storage/MarketplaceTracker.md), [ChannelRegistry](/contracts/channel/ChannelRegistry.md), [FeesInformation](/contracts/storage/FeesInformation.md), [ReinitializerGuard](/contracts/modules/library/ReinitializerGuard.md)

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

### initializeOfferCustody

Pins historically verified custody without changing ownership or moving offers/funds.

Called atomically through ProxyAdmin upgradeAndCall, or directly by the storage owner.
Existing ownership is retained. Version 2 must be unused; activation remains disabled.


```solidity
function initializeOfferCustody(address legacyRoyaltyGateway, address authorityGateway, address royaltyGateway)
    external
    reinitializer(2);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`legacyRoyaltyGateway`|`address`|Fixed, historically verified custody of all active untagged offers.|
|`authorityGateway`|`address`|Authorized creator of new id1 offers.|
|`royaltyGateway`|`address`|Authorized creator of new id2 offers; may equal the legacy gateway.|


### _hasReinitializerRole


```solidity
function _hasReinitializerRole(address caller) internal view override returns (bool);
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

