# RoyaltyTradeGateway
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/674fb60a18e2aa14b7080f0f43e11002723bd5b3/contracts/RoyaltyTradeGateway.sol)

**Inherits:**
Initializable, AccessControlUpgradeable, [ContractIntrospector](modules/library/ContractIntrospector.md), [ReinitializerGuard](modules/library/ReinitializerGuard.md), [TokenTradeModule](modules/trade/TokenTradeModule.md), [TradeRestrictionExtension](modules/trade/TradeRestrictionExtension.md)

**Title:**
RoyaltyTradeGateway

Entry point for all ERC-1155 token trade operations (listing, buying, offers).
`AccessToken(0x01)` trades are handled separately by the `AuthorityGateway` contract.

Upgradeable via `reinitializer(VERSION)`. Delegates state to the shared `IStorage` contract
and enforces trade-access restrictions through `TradeRestrictionExtension`.


## State Variables
### TRADE_ENTRY_REENTRANCY_GUARD_SLOT
Dedicated lock slot for external trade entrypoints (`buyToken`, `acceptOffer`).


```solidity
bytes32 private constant TRADE_ENTRY_REENTRANCY_GUARD_SLOT =
    keccak256("elacity.drm.royaltyTradeGateway.tradeEntry.reentrancy.v1")
```


### cstore
Reference to the central storage contract shared across the ecosystem.


```solidity
IStorage public cstore
```


## Functions
### tradeEntryNonReentrant

Prevents nested reentry into externally callable trade entrypoints.

Uses an isolated slot to avoid collisions with module-level guards.


```solidity
modifier tradeEntryNonReentrant() ;
```

### constructor

**Notes:**
- oz-upgrades-unsafe-allow: constructor

- docs-ignore: true


```solidity
constructor() ;
```

### _tradeEntryNonReentrantBefore


```solidity
function _tradeEntryNonReentrantBefore() internal;
```

### _tradeEntryNonReentrantAfter


```solidity
function _tradeEntryNonReentrantAfter() internal;
```

### initialize

**Notes:**
- docs-ignore: true

- oz-upgrades-validate-as-initializer: 


```solidity
function initialize(IStorage _dataStorage) public reinitializer(VERSION);
```

### _hasReinitializerRole


```solidity
function _hasReinitializerRole(address caller) internal view override returns (bool);
```

### sellersOf

Returns the list of addresses currently selling a given token.


```solidity
function sellersOf(address op) external view returns (address[] memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`op`|`address`|Address of the ERC-1155 contract (operative) that references the NFT.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address[]`|Array of seller addresses.|


### sellersOf

Returns the list of addresses currently selling a given token.


```solidity
function sellersOf(address channel, uint256 tokenId) external view returns (address[] memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`channel`|`address`|Address of the channel contract that holds the token.|
|`tokenId`|`uint256`|Token ID of the target NFT belonging to the channel.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address[]`|Array of seller addresses.|


### listings

Returns the listing details for a specific seller's token.


```solidity
function listings(address op, uint256 tkId, address seller) external view returns (uint256, uint256, address);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`op`|`address`|Address of the ERC-1155 contract that holds the token.|
|`tkId`|`uint256`|Token ID of the listed item.|
|`seller`|`address`|Address of the seller.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|quantity Number of tokens listed.|
|`<none>`|`uint256`|pricePerToken Price per single token in the smallest denomination.|
|`<none>`|`address`|payToken Address of the ERC-20 payment token (`address(0)` for native currency).|


### withdrawListing

Removes a specified quantity of tokens from the caller's active listing.


```solidity
function withdrawListing(address op, uint256 tkId, uint256 quantity) external override;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`op`|`address`|Address of the ERC-1155 contract that holds the token.|
|`tkId`|`uint256`|Token ID to delist.|
|`quantity`|`uint256`|Number of tokens to remove from the listing.|


### sellToken

Lists tokens for sale on the marketplace.

The caller must have granted ERC-1155 approval to this contract prior to calling.
The target contract must implement `ITradeAccessRestriction` and `IERC1155`.


```solidity
function sellToken(address _contract, uint256 tokenId, uint256 _quantity, uint256 _pricePerToken, address _payToken)
    external
    override
    restrictTradeOf(_contract, tokenId);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_contract`|`address`|Address of the ERC-1155 contract holding the tokens.|
|`tokenId`|`uint256`|Token ID to list for sale.|
|`_quantity`|`uint256`|Number of tokens to list.|
|`_pricePerToken`|`uint256`|Price per token in the smallest denomination of `_payToken`.|
|`_payToken`|`address`|Address of the ERC-20 payment token (`address(0)` for native currency).|


### buyToken

Purchases listed tokens from a seller.

For native-currency listings, `msg.value` must exactly match `pricePerToken * _quantity`.
For `ERC-20` listings, the buyer must have approved this contract to spend the required amount.


```solidity
function buyToken(address seller, address _contract, uint256 tokenId, uint256 _quantity)
    external
    payable
    override
    tradeEntryNonReentrant
    restrictTradeOf(_contract, tokenId);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`seller`|`address`|Address of the seller who listed the tokens.|
|`_contract`|`address`|Address of the ERC-1155 contract holding the tokens.|
|`tokenId`|`uint256`|Token ID to purchase.|
|`_quantity`|`uint256`|Number of tokens to buy.|


### createOffer

Creates a buy-offer for tokens using an `ERC-20` payment token.

The caller must have pre-approved this contract for the total ERC-20 amount.
Only one active offer per caller per token is allowed; cancel the existing offer first.


```solidity
function createOffer(
    address _contract,
    uint256 tokenId,
    uint256 _quantity,
    uint256 _pricePerToken,
    address payToken
) external override restrictTradeOf(_contract, tokenId);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_contract`|`address`|Address of the `ERC-1155` contract holding the tokens.|
|`tokenId`|`uint256`|Token ID to make an offer on.|
|`_quantity`|`uint256`|Number of tokens requested.|
|`_pricePerToken`|`uint256`|Offered price per token in `payToken` smallest denomination.|
|`payToken`|`address`|Address of the `ERC-20` token used for payment.|


### createOffer

Creates a buy-offer for tokens using native currency (`ETH`/`ELA`).

`msg.value` must exactly match `_pricePerToken * _quantity`.
The native currency is held in escrow by this contract until the offer is accepted or canceled.


```solidity
function createOffer(address _contract, uint256 tokenId, uint256 _quantity, uint256 _pricePerToken)
    external
    payable
    override
    restrictTradeOf(_contract, tokenId);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_contract`|`address`|Address of the `ERC-1155` contract holding the tokens.|
|`tokenId`|`uint256`|Token ID to make an offer on.|
|`_quantity`|`uint256`|Number of tokens requested.|
|`_pricePerToken`|`uint256`|Offered price per token in wei.|


### acceptOffer

Accepts a pending buy-offer, transferring tokens to the offerer and payment to the seller.

The caller (seller) must own and have approved the requested tokens.
Platform fees are deducted before the seller receives payment.


```solidity
function acceptOffer(address from, address _contract, uint256 tokenId, uint256 _quantity)
    external
    override
    tradeEntryNonReentrant
    restrictTradeOf(_contract, tokenId);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`from`|`address`|Address of the account that created the offer.|
|`_contract`|`address`|Address of the `ERC-1155` contract holding the tokens.|
|`tokenId`|`uint256`|Token ID the offer was made on.|
|`_quantity`|`uint256`|Number of tokens to sell into the offer (must be <= offered quantity).|


### cancelOffer

Cancels the caller's pending offer and refunds escrowed native currency (if any).

`ERC-20` offers do not require a refund since funds remain in the offerer's wallet.


```solidity
function cancelOffer(address _contract, uint256 tokenId) external override;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_contract`|`address`|Address of the `ERC-1155` contract the offer was made on.|
|`tokenId`|`uint256`|Token ID the offer was made on.|


## Errors
### ReentrantTradeEntryCall
Raised when a protected trade entrypoint is called reentrantly.


```solidity
error ReentrantTradeEntryCall();
```

### NativePaymentNotAccepted
Raised when native value is sent for an ERC-20 listing purchase.


```solidity
error NativePaymentNotAccepted(uint256 sent);
```

