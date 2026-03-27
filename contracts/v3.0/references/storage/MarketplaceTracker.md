# MarketplaceTracker
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/674fb60a18e2aa14b7080f0f43e11002723bd5b3/contracts/storage/MarketplaceTracker.sol)

**Inherits:**
[IMarketplaceTracker](/contracts/storage/IMarketplaceTracker.md), [ContractIntrospector](/contracts/modules/library/ContractIntrospector.md)

**Title:**
MarketplaceTracker

This abstract contract is used to track the marketplace listings and offers. It should be mounted on
`CoreStorage` contract.


## State Variables
### MARKETPLACE_TRACKER_STORAGE_LOCATION
Storage slot for MarketplaceTrackerStorage.
Formula: keccak256(abi.encode(uint256(keccak256("elacity.drm.storage.MarketplaceTracker")) - 1))
& ~bytes32(uint256(0xff))


```solidity
bytes32 private constant MARKETPLACE_TRACKER_STORAGE_LOCATION =
    0x50e5171928e9f9ca46bd1bf4edc7f04704965b985488c5b0645d7fbce2caeb00
```


## Functions
### owner

Owner resolver expected from inheriting storage root (e.g., CentralStorage).

**Note:**
docs-ignore: true


```solidity
function owner() public view virtual returns (address);
```

### _getMarketplaceTrackerStorage

Retrieves ERC-7201 namespaced storage.


```solidity
function _getMarketplaceTrackerStorage() private pure returns (MarketplaceTrackerStorage storage $);
```

### listings

Returns raw listing fields from storage.

keep on track of this issue https://github.com/ethereum/solidity/issues/6337
we need to define Listing here, not in the implementation


```solidity
function listings(address op, uint256 tokenId, address _owner)
    external
    view
    override
    returns (uint256, uint256, address);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`op`|`address`|Operative contract address.|
|`tokenId`|`uint256`|Asset token id.|
|`_owner`|`address`||

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Quantity, unit price, and payment token address.|
|`<none>`|`uint256`||
|`<none>`|`address`||


### offers

Returns raw offer fields from storage.


```solidity
function offers(address op, uint256 tokenId, address _from)
    external
    view
    override
    returns (uint256, uint256, address);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`op`|`address`|Operative contract address.|
|`tokenId`|`uint256`|Asset token id.|
|`_from`|`address`||

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Quantity, unit price, and payment token address.|
|`<none>`|`uint256`||
|`<none>`|`address`||


### _get32BytesKey

Calculate 32-bytes key for a given token

**Note:**
docs-ignore: true


```solidity
function _get32BytesKey(address op, uint256 tokenId) internal pure returns (bytes32);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`op`|`address`|address - Address of the Operative contract|
|`tokenId`|`uint256`|uint256 - Token ID of the asset|


### setListing

Sets listing fields for a seller.

Caller must hold `SystemRoles.ROLE_MARKET_WRITER`.

**Note:**
docs-ignore: true


```solidity
function setListing(
    address op,
    uint256 tokenId,
    address _owner,
    uint256 qt,
    uint256 pricePerToken,
    address payToken
) external whitelistRoleOnly(ISystemTracker(address(this)), SystemRoles.ROLE_MARKET_WRITER);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`op`|`address`|Operative contract address.|
|`tokenId`|`uint256`|Asset token id.|
|`_owner`|`address`||
|`qt`|`uint256`|Listed quantity.|
|`pricePerToken`|`uint256`|Unit price per token.|
|`payToken`|`address`|Payment token address.|


### getListing

**Note:**
docs-ignore: true


```solidity
function getListing(address op, uint256 tokenId, address _owner) external view returns (uint256, uint256, address);
```

### sellersOf

Get list of sellers for a given token


```solidity
function sellersOf(address op, uint256 tokenId) external view returns (address[] memory);
```

### setOffer

Sets offer fields for an offerer.

Caller must hold `SystemRoles.ROLE_MARKET_WRITER`.

**Note:**
docs-ignore: true


```solidity
function setOffer(address op, uint256 tokenId, address _from, uint256 qt, uint256 pricePerToken, address payToken)
    external
    whitelistRoleOnly(ISystemTracker(address(this)), SystemRoles.ROLE_MARKET_WRITER);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`op`|`address`|Operative contract address.|
|`tokenId`|`uint256`|Asset token id.|
|`_from`|`address`|Offerer address.|
|`qt`|`uint256`|Offered quantity.|
|`pricePerToken`|`uint256`|Unit price per token.|
|`payToken`|`address`|Payment token address.|


### getOffer

Get offer for a given token


```solidity
function getOffer(address op, uint256 tokenId, address _from) external view returns (uint256, uint256, address);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`op`|`address`|address - Address of the Operative contract|
|`tokenId`|`uint256`|uint256 - Token ID of the asset|
|`_from`|`address`|address - Address of the offerer|


### offerersOf

Get list of offerers for a given token


```solidity
function offerersOf(address op, uint256 tokenId) external view returns (address[] memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`op`|`address`|address - Address of the Operative contract|
|`tokenId`|`uint256`|uint256 - Token ID of the asset|


### setTaxInformation

Updates platform fee configuration.

Only storage owner can mutate tax configuration.
`_platformFee` is expressed over 1000 units where 1000 = 100%.


```solidity
function setTaxInformation(uint16 _platformFee, address _feeRecipient) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_platformFee`|`uint16`|Fee value in basis over 1000.|
|`_feeRecipient`|`address`|Recipient for protocol fee payouts.|


### taxInformation

Returns current platform fee settings.


```solidity
function taxInformation() external view returns (uint16, address);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint16`|Fee in basis points over 1000.|
|`<none>`|`address`|Recipient of the platform fee.|


## Errors
### TaxOverflowError
Thrown when the platform fee exceeds 100%.


```solidity
error TaxOverflowError(uint256 platformFee);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`platformFee`|`uint256`|The platform fee value that caused the error.|

### InvalidFeeRecipient
Thrown when an invalid fee recipient address is provided.


```solidity
error InvalidFeeRecipient(address recipient);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`recipient`|`address`|The invalid fee recipient address.|

### UnauthorizedTaxSetter
Thrown when a caller without the necessary permissions attempts to set the platform fee.


```solidity
error UnauthorizedTaxSetter(address caller);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`caller`|`address`|The address that attempted to set the platform fee.|

## Structs
### Listing
Structure to hold listing details


```solidity
struct Listing {
    // uint256 tokenId; // ACCESS_TOKEN | ROYALTY_ShARE | DISTRIBUTION_RIGHT |...
    uint256 quantity;
    uint256 pricePerToken;
    address payToken; // accepts Native or ERC20
}
```

**Properties**

|Name|Type|Description|
|----|----|-----------|
|`quantity`|`uint256`|uint256 - Quantity of tokens being listed|
|`pricePerToken`|`uint256`|uint256 - Price per token|
|`payToken`|`address`|address - Address of the token to be paid|

### Offer
Structure to hold offer details


```solidity
struct Offer {
    uint256 quantity;
    uint256 pricePerToken;
    address payToken;
}
```

**Properties**

|Name|Type|Description|
|----|----|-----------|
|`quantity`|`uint256`|uint256 - Quantity of tokens being offered|
|`pricePerToken`|`uint256`|uint256 - Price per token|
|`payToken`|`address`|address - Address of the token to be paid|

### MarketplaceTrackerStorage
**Note:**
storage-location: erc7201:elacity.drm.storage.MarketplaceTracker


```solidity
struct MarketplaceTrackerStorage {
    /**
     * @notice Amount in percentage of fee Elacity will receive on each trade.
     * 1000 units of this value corresponds to 100%.
     */
    uint16 platformFee;
    /**
     * @notice Address of the fee recipient.
     */
    address feeRecipient;
    /// @notice map of listings: op -> tokenId -> owner -> Listing details.
    mapping(address => mapping(uint256 => mapping(address => Listing))) listings;
    /// @notice Map of sellers for a given token (key: keccak256(op, tokenId)).
    mapping(bytes32 => EnumerableSet.AddressSet) sellers;
    /// @notice Map of offers for a given token (op -> tokenId -> offerer -> Offer).
    mapping(address => mapping(uint256 => mapping(address => Offer))) offers;
    /// @notice Map of offerers for a given token (key: keccak256(op, tokenId)).
    mapping(bytes32 => EnumerableSet.AddressSet) offerers;
}
```

