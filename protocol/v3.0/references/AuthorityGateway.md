# AuthorityGateway
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/bc1f2ea3fd5d8b703627a7946e7d5fe7fb13f047/contracts/AuthorityGateway.sol)

**Inherits:**
Initializable, [ReinitializerGuard](/contracts/modules/library/ReinitializerGuard.md), [ProtocolVersioned](/contracts/library/ProtocolVersioned.md), AccessControlUpgradeable, [ContractIntrospector](/contracts/modules/library/ContractIntrospector.md), [AccessOfferModule](/contracts/modules/trade/AccessOfferModule.md)

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
Shared entry lock for buyAccess and all access-offer mutations.


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

Prevents nested checkout and access-offer entry across all payment/token callbacks.

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


### createOffer

Creates an ERC-20 offer; funds remain with the maker until acceptance.

No expiry or replacement. Approve Authority for direct fees and the operative processor for routed payouts.


```solidity
function createOffer(address op, uint256 tokenId, uint256 quantity, uint256 price, address payToken)
    external
    override
    buyAccessNonReentrant;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`op`|`address`||
|`tokenId`|`uint256`|Must equal ACCESS_TOKEN (1), not the channel media id.|
|`quantity`|`uint256`||
|`price`|`uint256`||
|`payToken`|`address`|Nonzero ERC-20 address; the native sentinel is rejected.|


### createOffer

Creates an exactly funded native offer held by this gateway.

msg.value must equal quantity times unit price. Funds stay escrowed until fill or maker cancellation.


```solidity
function createOffer(address op, uint256 tokenId, uint256 quantity, uint256 price)
    external
    payable
    override
    buyAccessNonReentrant;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`op`|`address`||
|`tokenId`|`uint256`|Must equal ACCESS_TOKEN (1).|
|`quantity`|`uint256`||
|`price`|`uint256`||


### createAccessOffer

Creates an ERC-20 access offer for the operative resolved from a channel media id.


```solidity
function createAccessOffer(address ledger, uint256 mediaId, uint256 quantity, uint256 price, address payToken)
    external
    buyAccessNonReentrant;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`ledger`|`address`|Channel address registered in storage.|
|`mediaId`|`uint256`|Channel media id; emitted/stored token identity is the resolved operative and id1.|
|`quantity`|`uint256`|Positive number of existing access tokens requested.|
|`price`|`uint256`|Positive unit price in smallest payment-token units.|
|`payToken`|`address`|Nonzero ERC-20 asset; approve Authority for fees and the operative processor for routed funds.|


### createAccessOffer

Creates an exactly funded native access offer for a channel media id.


```solidity
function createAccessOffer(address ledger, uint256 mediaId, uint256 quantity, uint256 price)
    external
    payable
    buyAccessNonReentrant;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`ledger`|`address`|Channel address registered in storage.|
|`mediaId`|`uint256`|Channel media id, resolved to the operative id1 offer key.|
|`quantity`|`uint256`|Positive requested quantity, without expiry.|
|`price`|`uint256`|Native unit price; msg.value must equal quantity times price.|


### acceptAccessOffer

Accepts an access offer resolved from a channel media id with expected-term protection.


```solidity
function acceptAccessOffer(
    address from,
    address ledger,
    uint256 mediaId,
    uint256 quantity,
    uint256 expectedPricePerToken,
    address expectedPayToken
) external buyAccessNonReentrant;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`from`|`address`|Maker, payer and access-token recipient.|
|`ledger`|`address`|Registered channel containing the media.|
|`mediaId`|`uint256`|Media id used to resolve the operative.|
|`quantity`|`uint256`|Positive fill quantity; caller owns and approves these tokens.|
|`expectedPricePerToken`|`uint256`|Exact agreed unit price, including across cancel/recreate races.|
|`expectedPayToken`|`address`|Exact agreed payment token; zero represents native escrow.|


### acceptOffer

Accepts current terms only if price and payment token match the seller's expectations.

Caller is seller; maker pays and receives tokens. Current OP1 owner only; eligible OP2 holders may resell.


```solidity
function acceptOffer(
    address from,
    address op,
    uint256 tokenId,
    uint256 quantity,
    uint256 expectedPricePerToken,
    address expectedPayToken
) external override buyAccessNonReentrant;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`from`|`address`|Offer maker and recipient of the tokens.|
|`op`|`address`||
|`tokenId`|`uint256`|Must equal ACCESS_TOKEN (1).|
|`quantity`|`uint256`||
|`expectedPricePerToken`|`uint256`|Exact agreed unit price; rejects changed terms on recreation, but identical terms may still fill a recreated offer.|
|`expectedPayToken`|`address`|Exact payment asset agreed to; zero means native currency.|


### cancelOffer

Cancels the maker's own offer and atomically refunds any remaining native escrow.

No current operative eligibility check; a rejecting refund receiver restores the whole offer.


```solidity
function cancelOffer(address op, uint256 tokenId) external override buyAccessNonReentrant;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`op`|`address`||
|`tokenId`|`uint256`|Must equal ACCESS_TOKEN (1).|


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

