# OperativePrimitive
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/52ca0e7824ef5fab5ebe0a131f7c6e6dd330de09/contracts/operative/kind/OperativePrimitive.sol)

**Inherits:**
[IOperative](/contracts/operative/IOperative.md), ERC1155SupplyUpgradeable, [OwnableExclusiveTransferrableTokens](/contracts/modules/library/OwnableExclusiveTransferrableTokens.md), [TradeAccessRestriction](/contracts/modules/trade/TradeAccessRestriction.md), [ContractIntrospector](/contracts/modules/library/ContractIntrospector.md), [RewardsRecipient](/contracts/modules/payment/RewardsRecipient.md), [RoyaltyModule](/contracts/modules/royalty/RoyaltyModule.md), [ProtocolVersioned](/contracts/library/ProtocolVersioned.md)

**Title:**
OperativePrimitive

Base ERC-1155 contract for all operative types in the Elacity DRM ecosystem.
An operative represents the access-control and royalty layer that sits beneath a digital
asset. Three built-in token IDs govern behaviour:
- `ACCESS_TOKEN` (1) – grants playback / consumption rights.
- `ROYALTY_SHARE` (2) – entitles the holder to a proportional share of sale revenue.
- `DISTRIBUTION_RIGHT` (3) – authorises the holder to sell or trade access tokens.

Upgradeable via beacon proxy. Concrete implementations (`OperativeBuyable`,
`OperativeBuyableSellable`) inherit this contract and define their own `OP_TYPE`.


## State Variables
### cstore
Reference to the shared ecosystem storage contract.


```solidity
IStorage public cstore
```


### contentId
Unique identifier linking this operative to its parent digital asset.


```solidity
bytes16 public contentId
```


### OPERATIVE_PRIMITIVE_STORAGE_SLOT

```solidity
bytes32 private constant OPERATIVE_PRIMITIVE_STORAGE_SLOT =
    0xac9fbf24d236d37ace6450911d25f79cda65e600e93cea5f200c122b53978400
```


## Functions
### _getOperativePrimitiveStorage


```solidity
function _getOperativePrimitiveStorage() internal pure returns (OperativePrimitiveStorage storage $);
```

### accessTransferAuthorized

Reverts with `UnauthorizedTransfer` when `from` lacks the
`DISTRIBUTION_RIGHT` required to transfer access tokens.


```solidity
modifier accessTransferAuthorized(address from, uint256[] memory ids) ;
```

### _requireAccessTransferAuthorized


```solidity
function _requireAccessTransferAuthorized(address from, uint256[] memory ids) internal view;
```

### constructor

**Notes:**
- oz-upgrades-unsafe-allow: constructor

- docs-ignore: true


```solidity
constructor() ;
```

### initialize

Initialises the operative with its parent storage, content identifier, and token-metadata URI.


```solidity
function initialize(IStorage _dataStorage, bytes16 _contentId, string calldata baseURI) public virtual initializer;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_dataStorage`|`IStorage`|Shared ecosystem storage contract.|
|`_contentId`|`bytes16`|  Unique identifier that links this operative to its digital asset.|
|`baseURI`|`string`|     Base URI for ERC-1155 token metadata.|


### setupDistributionRights

Mints a single `DISTRIBUTION_RIGHT` token to the content creator.

Must be called by the factory immediately after proxy initialisation.


```solidity
function setupDistributionRights(address creator) external onlyOwner;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`creator`|`address`|Address that will receive the distribution-right token.|


### _accessTransferAuthorized

Checks if the caller is authorized to transfer the specified tokens.


```solidity
function _accessTransferAuthorized(address from, uint256[] memory ids) internal view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`from`|`address`|The address to check.|
|`ids`|`uint256[]`|The token IDs to check.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if the caller is authorized, false otherwise.|


### mintOperativeTokens

Mints one token per entry, allowing different recipients in a single call.

Each index across `to`, `ids`, and `amounts` forms a single mint instruction.


```solidity
function mintOperativeTokens(address[] memory to, uint256[] memory ids, uint256[] memory amounts, bytes memory data)
    public
    onlyOwner;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`to`|`address[]`|     Array of recipient addresses.|
|`ids`|`uint256[]`|    Array of token IDs to mint.|
|`amounts`|`uint256[]`|Array of quantities to mint.|
|`data`|`bytes`|   Optional data forwarded to `_mint`.|


### _mintProtocolRoyaltyShareOnce

Mints configured protocol royalty shares on initial royalty distribution.


```solidity
function _mintProtocolRoyaltyShareOnce(bool mintedRoyaltyShare) internal;
```

### _checkAccessFor

Checks the access level for the specified account and token IDs.


```solidity
function _checkAccessFor(address account, uint256[] memory ids) internal view returns (AccessLevel[] memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`account`|`address`|The address to check.|
|`ids`|`uint256[]`|The token IDs to check.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`AccessLevel[]`|An array of AccessLevel structs.|


### checkAccess

Returns the access levels for `account`, checking `ACCESS_TOKEN` and `DISTRIBUTION_RIGHT`.


```solidity
function checkAccess(address account) external view virtual returns (AccessLevel[] memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`account`|`address`|Address to query.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`AccessLevel[]`|Array of `AccessLevel` structs for each checked token.|


### protocolVersion

Returns the protocol major/minor version this operative implementation targets.


```solidity
function protocolVersion() public pure virtual override(IOperative, ProtocolVersioned) returns (string memory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`string`|Version string in `major.minor` format (for example `3.0`).|


### uri

Returns the metadata URI for a specific token ID.

Appends `/{id}.json` to the base URI and resolves `{id}` using ERC-1155
metadata substitution rules (lowercase 64-char hex, no `0x`).


```solidity
function uri(uint256 id) public view override(ERC1155Upgradeable) returns (string memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`id`|`uint256`|Token ID to query.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`string`|Fully-qualified metadata URI.|


### tokenURI

Returns the resolved metadata URI for a specific token ID.


```solidity
function tokenURI(uint256 tokenId) public view virtual returns (string memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenId`|`uint256`|Token ID to query.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`string`|Fully-qualified metadata URI.|


### royaltyInfo

Computes the royalty distribution for a given sale price.

Each holder's share is proportional to their `ROYALTY_SHARE` balance relative to
the total supply of that token.


```solidity
function royaltyInfo(uint256 _salePrice) external view returns (RoyaltyInfo[] memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_salePrice`|`uint256`|Total sale price to distribute royalties from.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`RoyaltyInfo[]`|Array of `RoyaltyInfo` structs with each holder's address and owed amount.|


### _remapRoyaltyHoldings

Remaps royalty holdings from one address to another. This ensures easy way to retrieve
the list of royalty holders.


```solidity
function _remapRoyaltyHoldings(address from, address to, uint256 amount) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`from`|`address`|The address to remove.|
|`to`|`address`|The address to add.|
|`amount`|`uint256`|The amount to transfer.|


### hasTradeAccess

Determines whether `account` is allowed to trade the token identified by `tkId`.

For `ROYALTY_SHARE` trades, the caller must hold either an access token or a royalty
share. All other token IDs are permitted by default (subject to `OwnableExclusiveTransferrableTokens`).


```solidity
function hasTradeAccess(address account, uint256 tkId) public view override returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`account`|`address`|Address to check.|
|`tkId`|`uint256`|   Token ID being traded.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|`true` if the account may trade the specified token.|


### metadataURI

Returns the contract-level metadata URI, shared with the parent digital asset.


```solidity
function metadataURI() external view returns (string memory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`string`|URI pointing to `contract.json`.|


### setPaymentProcessor

Restrict payment processor changes to the contract owner


```solidity
function setPaymentProcessor(address _payProc) external override onlyOwner;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_payProc`|`address`|Address of the payment processor contract|


### _checkOwnerLater

Allows acknowledged ecosystem contracts to authorize transfers on operatives


```solidity
function _checkOwnerLater() internal view override;
```

### supportsInterface


```solidity
function supportsInterface(bytes4 interfaceId)
    public
    view
    virtual
    override(IERC165, ERC1155Upgradeable, TradeAccessRestriction)
    returns (bool);
```

### _update


```solidity
function _update(address from, address to, uint256[] memory ids, uint256[] memory values)
    internal
    virtual
    override;
```

### receive

Allows the operative to receive native currency (ETH / ELA) for payment processing.


```solidity
receive() external payable virtual;
```

## Errors
### OperativeTokensAlreadyMinted
Thrown when attempting to run initial operative minting more than once.


```solidity
error OperativeTokensAlreadyMinted();
```

## Structs
### OperativePrimitiveStorage
**Note:**
storage-location: erc7201:elacity.drm.storage.OperativePrimitive


```solidity
struct OperativePrimitiveStorage {
    /// @dev Set of addresses that currently hold `ROYALTY_SHARE` tokens.
    EnumerableSet.AddressSet royaltyHolders;
    /// @dev Tracks whether initial operative token mint has already been processed.
    bool operativeTokensMinted;
}
```

