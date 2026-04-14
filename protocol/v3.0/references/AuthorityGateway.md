# AuthorityGateway
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/a429f79c38ae4f5221da86eca62d9868f0a5a7fd/contracts/AuthorityGateway.sol)

**Inherits:**
Initializable, [ReinitializerGuard](/contracts/modules/library/ReinitializerGuard.md), [ProtocolVersioned](/contracts/library/ProtocolVersioned.md), AccessControlUpgradeable, [ContractIntrospector](/contracts/modules/library/ContractIntrospector.md), [AccessTradeModule](/contracts/modules/trade/AccessTradeModule.md)

**Title:**
AuthorityGateway

Main front-facing contract that governs access to digital media.
It allows selling and buying access tokens, and checking access rights
for specific digital assets.

About versioning and the `reinitializer(uint64)` modifier:
It is an increasing number and contains 8-bytes to comply with
`reinitializer(uint64)` modifier of the `Initializable` contract.
How it is formed?*
- version of Authority gateway: eg. 2.0 -> [0x02, 0x00]
- deployment version eg: (ecosystem iteration) 0.6.0: [0x00, 0x06, 0x00]
- first 3 bytes are reserved for future use and ensure it keeps increasing


## State Variables
### BUY_ACCESS_REENTRANCY_GUARD_SLOT
Dedicated reentrancy slot for `buyAccess` externals only.


```solidity
bytes32 private constant BUY_ACCESS_REENTRANCY_GUARD_SLOT =
    keccak256("elacity.drm.authorityGateway.buyAccess.reentrancy.v1")
```


### cstore
Data storage contract.


```solidity
IStorage public cstore
```


## Functions
### buyAccessNonReentrant

Prevents nested external `buyAccess` entry.

Uses an isolated storage slot so it does not conflict with royalty payout reentrancy guards.


```solidity
modifier buyAccessNonReentrant() ;
```

### constructor

**Note:**
oz-upgrades-unsafe-allow: constructor


```solidity
constructor() ;
```

### initialize

**Notes:**
- docs-ignore: true

- oz-upgrades-validate-as-initializer: 


```solidity
function initialize(IStorage _dataStorage) public reinitializer(VERSION);
```

### _buyAccessNonReentrantBefore


```solidity
function _buyAccessNonReentrantBefore() internal;
```

### _buyAccessNonReentrantAfter


```solidity
function _buyAccessNonReentrantAfter() internal;
```

### _hasReinitializerRole

Checks if the caller has the reinitializer role.


```solidity
function _hasReinitializerRole(address caller) internal view override returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`caller`|`address`|Address of the caller.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if the caller has the reinitializer role, false otherwise.|


### sellAccess

Sell access tokens.


```solidity
function sellAccess(address ledger, uint256 tokenId, uint256 _quantity, uint256 _pricePerToken, address _payToken)
    external
    override;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`ledger`|`address`|Address of the ledger contract.|
|`tokenId`|`uint256`|Token ID of the access token.|
|`_quantity`|`uint256`|Quantity of access tokens to sell.|
|`_pricePerToken`|`uint256`|Price per access token.|
|`_payToken`|`address`|Address of the token to be paid.|


### sellAccessOnBehalf

Sell access tokens on behalf of another address.
Only an acknowledged contract can call this method.


```solidity
function sellAccessOnBehalf(
    address seller,
    address ledger,
    uint256 tokenId,
    uint256 _quantity,
    uint256 _pricePerToken,
    address _payToken
) external override whitelistOnly(cstore);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`seller`|`address`|Address of the seller.|
|`ledger`|`address`|Address of the ledger contract.|
|`tokenId`|`uint256`|Token ID of the access token.|
|`_quantity`|`uint256`|Quantity of access tokens to sell.|
|`_pricePerToken`|`uint256`|Price per access token.|
|`_payToken`|`address`|Address of the token to be paid.|


### buyAccess

Buy access tokens with native currency.


```solidity
function buyAccess(address seller, address ledger, uint256 tokenId, uint256 _quantity, uint256 _pricePerToken)
    external
    payable
    override
    buyAccessNonReentrant;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`seller`|`address`|Address of the seller.|
|`ledger`|`address`|Address of the ledger contract.|
|`tokenId`|`uint256`|Token ID of the access token.|
|`_quantity`|`uint256`|Quantity of access tokens to buy.|
|`_pricePerToken`|`uint256`|Price per access token.|


### buyAccess

Buy access tokens with an ERC-20 payment token.


```solidity
function buyAccess(
    address seller,
    address ledger,
    uint256 tokenId,
    uint256 _quantity,
    uint256 _pricePerToken,
    address _payToken
) external override buyAccessNonReentrant;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`seller`|`address`|Address of the seller.|
|`ledger`|`address`|Address of the ledger contract.|
|`tokenId`|`uint256`|Token ID of the access token.|
|`_quantity`|`uint256`|Quantity of access tokens to buy.|
|`_pricePerToken`|`uint256`|Price per access token.|
|`_payToken`|`address`|Address of the token to be paid.|


### withdrawListing

Withdraw listing from the marketplace.


```solidity
function withdrawListing(address op, uint256 tokenId, uint256 quantity) external override;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`op`|`address`|Address of the operative contract.|
|`tokenId`|`uint256`|Token ID of the access token.|
|`quantity`|`uint256`|Quantity of access tokens to withdraw.|


### hasAccess

Check if an address has access to the given token.


```solidity
function hasAccess(address accessor, address ledger, uint256 tokenId) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`accessor`|`address`|The accessor address.|
|`ledger`|`address`|The ledger address.|
|`tokenId`|`uint256`|The token ID.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if the accessor has access, false otherwise.|


### hasAccessByContentId

Check whether an accessor address has access to a media referenced by its content ID.


```solidity
function hasAccessByContentId(address accessor, bytes16 contentId) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`accessor`|`address`|The accessor address.|
|`contentId`|`bytes16`|The content ID of the media.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if the accessor has access, false otherwise.|


### _checkUserAccess

Checks whether an accessor has access to a digital asset. Access is granted
if the user holds an access-granting token on the operative OR has an active
subscription on the channel (including token-gated access and multi-channel
parent propagation).


```solidity
function _checkUserAccess(address accessor, address ledger, uint256 tokenId) internal view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`accessor`|`address`|The address to check.|
|`ledger`|`address`|The ledger (channel) contract address.|
|`tokenId`|`uint256`|The token ID within the ledger.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if the accessor has access through any path.|


### _getOperative

Get the operative contract for a given ledger and token ID.


```solidity
function _getOperative(address ledger, uint256 tokenId) internal view returns (address, IOperative);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`ledger`|`address`|Address of the ledger contract.|
|`tokenId`|`uint256`|Token ID of the access token.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|_op Address of the operative contract.|
|`<none>`|`IOperative`|Instance of the operative contract.|


### operative

Get the operative contract address for a given ledger and token ID.


```solidity
function operative(address ledger, uint256 tokenId) external view returns (address);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`ledger`|`address`|Address of the ledger contract.|
|`tokenId`|`uint256`|Token ID of the access token.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|Address of the operative contract.|


### sellersOf

Get the sellers of a given operative contract and token ID.


```solidity
function sellersOf(address op, uint256 tokenId) external view override returns (address[] memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`op`|`address`|Address of the operative contract.|
|`tokenId`|`uint256`|Token ID of the access token.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address[]`|Array of sellers.|


### listings

Get the listing for a given operative contract and token ID.


```solidity
function listings(address op, uint256 tokenId, address seller)
    external
    view
    override
    returns (uint256, uint256, address);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`op`|`address`|Address of the operative contract.|
|`tokenId`|`uint256`|Token ID of the access token.|
|`seller`|`address`|Address of the seller.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|quantity Quantity of access tokens.|
|`<none>`|`uint256`|pricePerToken Price per access token.|
|`<none>`|`address`|payToken Address of the token to be paid.|


### supportsLitProtocol

Check if the Authority Gateway supports the Lit Protocol
CEK bindings. Only recent versions should have it.


```solidity
function supportsLitProtocol() external pure returns (bool);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if the Authority Gateway supports the Lit Protocol, false otherwise.|


## Errors
### UnboundContentId
Thrown when a content ID is not bound to any channel/token pair.


```solidity
error UnboundContentId(bytes16 contentId);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`contentId`|`bytes16`|Content ID of the media.|

