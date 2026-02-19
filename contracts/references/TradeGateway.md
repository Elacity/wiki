## TradeGateway

Entry point for all ERC-1155 token trade operations (listing, buying, offers).
`AccessToken(0x01)` trades are handled separately by the `AuthorityGateway` contract.

_Upgradeable via `reinitializer(VERSION)`. Delegates state to the shared `IStorage` contract
and enforces trade-access restrictions through `TradeRestrictionExtension`._

### store

```solidity
contract IStorage store
```

Reference to the central storage contract shared across the ecosystem.

### sellersOf

```solidity
function sellersOf(address op, uint256 tkId) external view returns (address[])
```

Returns the list of addresses currently selling a given token.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| op | address | Address of the ERC-1155 contract (operative or channel) that holds the token |
| tkId | uint256 | Token ID to query sellers for |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | address[] | Array of seller addresses |

### listings

```solidity
function listings(address op, uint256 tkId, address seller) external view returns (uint256, uint256, address)
```

Returns the listing details for a specific seller's token.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| op | address | Address of the ERC-1155 contract that holds the token |
| tkId | uint256 | Token ID of the listed item |
| seller | address | Address of the seller |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | uint256 | quantity Number of tokens listed |
| [1] | uint256 | pricePerToken Price per single token in the smallest denomination |
| [2] | address | payToken Address of the ERC-20 payment token (`address(0)` for native currency) |

### withdrawListing

```solidity
function withdrawListing(address op, uint256 tkId, uint256 quantity) external
```

Removes a specified quantity of tokens from the caller's active listing.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| op | address | Address of the ERC-1155 contract that holds the token |
| tkId | uint256 | Token ID to delist |
| quantity | uint256 | Number of tokens to remove from the listing |

### sellToken

```solidity
function sellToken(address _contract, uint256 tokenId, uint256 _quantity, uint256 _pricePerToken, address _payToken) external
```

Lists tokens for sale on the marketplace.

_The caller must have granted ERC-1155 approval to this contract prior to calling.
The target contract must implement `ITradeAccessRestriction` and `IERC1155`._

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| _contract | address | Address of the ERC-1155 contract holding the tokens |
| tokenId | uint256 | Token ID to list for sale |
| _quantity | uint256 | Number of tokens to list |
| _pricePerToken | uint256 | Price per token in the smallest denomination of `_payToken` |
| _payToken | address | Address of the ERC-20 payment token (`address(0)` for native currency) |

### buyToken

```solidity
function buyToken(address seller, address _contract, uint256 tokenId, uint256 _quantity) external payable
```

Purchases listed tokens from a seller.

_For native-currency listings, `msg.value` must cover `pricePerToken * _quantity`.
For `ERC-20` listings, the buyer must have approved this contract to spend the required amount._

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| seller | address | Address of the seller who listed the tokens |
| _contract | address | Address of the ERC-1155 contract holding the tokens |
| tokenId | uint256 | Token ID to purchase |
| _quantity | uint256 | Number of tokens to buy |

### createOffer

```solidity
function createOffer(address _contract, uint256 tokenId, uint256 _quantity, uint256 _pricePerToken, address payToken) external
```

Creates a buy-offer for tokens using an `ERC-20` payment token.

_The caller must have pre-approved this contract for the total ERC-20 amount.
Only one active offer per caller per token is allowed; cancel the existing offer first._

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| _contract | address | Address of the `ERC-1155` contract holding the tokens |
| tokenId | uint256 | Token ID to make an offer on |
| _quantity | uint256 | Number of tokens requested |
| _pricePerToken | uint256 | Offered price per token in `payToken` smallest denomination |
| payToken | address | Address of the `ERC-20` token used for payment |

### createOffer

```solidity
function createOffer(address _contract, uint256 tokenId, uint256 _quantity, uint256 _pricePerToken) external payable
```

Creates a buy-offer for tokens using native currency (`ETH`/`ELA`).

_`msg.value` must be at least `_pricePerToken * _quantity`. Excess value is not refunded.
The native currency is held in escrow by this contract until the offer is accepted or canceled._

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| _contract | address | Address of the `ERC-1155` contract holding the tokens |
| tokenId | uint256 | Token ID to make an offer on |
| _quantity | uint256 | Number of tokens requested |
| _pricePerToken | uint256 | Offered price per token in wei |

### acceptOffer

```solidity
function acceptOffer(address from, address _contract, uint256 tokenId, uint256 _quantity) external
```

Accepts a pending buy-offer, transferring tokens to the offerer and payment to the seller.

_The caller (seller) must own and have approved the requested tokens.
Platform fees are deducted before the seller receives payment._

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| from | address | Address of the account that created the offer |
| _contract | address | Address of the `ERC-1155` contract holding the tokens |
| tokenId | uint256 | Token ID the offer was made on |
| _quantity | uint256 | Number of tokens to sell into the offer (must be <= offered quantity) |

### cancelOffer

```solidity
function cancelOffer(address _contract, uint256 tokenId) external
```

Cancels the caller's pending offer and refunds escrowed native currency (if any).

_`ERC-20` offers do not require a refund since funds remain in the offerer's wallet._

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| _contract | address | Address of the `ERC-1155` contract the offer was made on |
| tokenId | uint256 | Token ID the offer was made on |

