## ITradableErrors

### ZeroValueError

```solidity
error ZeroValueError()
```

### NotAllowedError

```solidity
error NotAllowedError(address from)
```

### NotApprovedError

```solidity
error NotApprovedError(address _operative)
```

### InvalidOperativeError

```solidity
error InvalidOperativeError(address _operative)
```

### InvalidPaymentTokenError

```solidity
error InvalidPaymentTokenError(address _payToken)
```

### InsufficientOwningError

```solidity
error InsufficientOwningError(address _operative, address _owner, uint256 _balance, uint256 _qt)
```

### PriceFulfillmentError

```solidity
error PriceFulfillmentError(uint256 value, uint256 toPay)
```

### AvailabilityError

```solidity
error AvailabilityError(uint256 actual, uint256 requested)
```

Indicates the withdrawal quantity is higher than listed

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| actual | uint256 | Amount of tokens stated |
| requested | uint256 | Amount of tokens requested |

### NoOverrideError

```solidity
error NoOverrideError(address from)
```

prevents override of a trade terms

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| from | address | The address that called the function |

## ITradableEvents

### ItemListed

```solidity
event ItemListed(address seller, address op, uint256 tkId, uint256 quantity, uint256 pricePerToken, address payToken)
```

### ItemSold

```solidity
event ItemSold(address seller, address buyer, address op, uint256 tkId, address payToken, uint256 unitPrice, uint256 price)
```

### ItemUnlisted

```solidity
event ItemUnlisted(address seller, address op, uint256 tkId, uint256 quantity)
```

## ITradable

### withdrawListing

```solidity
function withdrawListing(address op, uint256 tokenId, uint256 quantity) external
```

_Allows a seller to withdraw token from listing. the quantity should be less than or
equal to the quantity listed_

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| op | address | The address of the target operative contract |
| tokenId | uint256 | The id of the token |
| quantity | uint256 | The quantity to withdraw |

### listings

```solidity
function listings(address op, uint256 tokenId, address seller) external view returns (uint256, uint256, address)
```

_Get the listing details of a digital asset, the asset here is defined directly
from its location within the Operative contract_

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| op | address | The address of the operative |
| tokenId | uint256 | The id of the token |
| seller | address | The address of the seller |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | uint256 | The quantity, price per token and the payment token address |
| [1] | uint256 |  |
| [2] | address |  |

### sellersOf

```solidity
function sellersOf(address op, uint256 tokenId) external view returns (address[])
```

_Get the sellers of a digital asset, the asset here is defined within the operative contract context_

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| op | address | The address of the operative |
| tokenId | uint256 | The id of the token |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | address[] | The sellers of the token |

## ITokenTradable

### sellToken

```solidity
function sellToken(address _contract, uint256 tokenId, uint256 _quantity, uint256 _pricePerToken, address _payToken) external
```

### buyToken

```solidity
function buyToken(address seller, address _contract, uint256 tokenId, uint256 _quantity) external payable
```

## ITokenOfferable

### OfferSettled

```solidity
event OfferSettled(address from, address _contract, uint256 tokenId, uint256 _quantity, uint256 _pricePerToken, address payToken)
```

### OfferCanceled

```solidity
event OfferCanceled(address from, address _contract, uint256 tokenId)
```

### OfferAccepted

```solidity
event OfferAccepted(address by, address from, address _contract, uint256 tokenId, uint256 _quantity, uint256 _pricePerToken, address payToken)
```

### createOffer

```solidity
function createOffer(address _contract, uint256 tokenId, uint256 _quantity, uint256 _pricePerToken, address payToken) external
```

### acceptOffer

```solidity
function acceptOffer(address from, address _contract, uint256 tokenId, uint256 _quantity) external
```

### cancelOffer

```solidity
function cancelOffer(address _contract, uint256 tokenId) external
```

## IAccessTradable

### sellAccess

```solidity
function sellAccess(address ledger, uint256 tokenId, uint256 _quantity, uint256 _pricePerToken, address _payToken) external
```

_Put a digital asset on sale, the asset here is defined by its location in the ledger context_

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| ledger | address | The address of the ledger |
| tokenId | uint256 | The id of the token |
| _quantity | uint256 | The quantity of the token |
| _pricePerToken | uint256 | The price per token |
| _payToken | address | The address of the token to pay with |

### sellAccessOnBehalf

```solidity
function sellAccessOnBehalf(address seller, address ledger, uint256 tokenId, uint256 _quantity, uint256 _pricePerToken, address _payToken) external
```

_Put a digital asset on sale on behalf of a user through a non-EOA address, the asset here is defined by its location in the ledger context
the exection could require ERC1155 approval from the seller to succeed_

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| seller | address | The address of the seller |
| ledger | address | The address of the ledger |
| tokenId | uint256 | The id of the token |
| _quantity | uint256 | The quantity of the token |
| _pricePerToken | uint256 | The price per token |
| _payToken | address | The address of the token to pay with |

### buyAccess

```solidity
function buyAccess(address seller, address ledger, uint256 tokenId, uint256 _quantity, uint256 _pricePerToken) external payable
```

This moethod requires ERC1155 approval from the buyer in prior to the execution

_Buy a digital asset, the asset here is defined by its location in the ledger context.
The amount that should be passed into `msg.value` should fulfill the operation and should not be less than _quantity * _pricePerToken_

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| seller | address | The address of the seller |
| ledger | address | The address of the ledger |
| tokenId | uint256 | The id of the token |
| _quantity | uint256 | The quantity of the token |
| _pricePerToken | uint256 | The price per token |

### buyAccess

```solidity
function buyAccess(address seller, address ledger, uint256 tokenId, uint256 _quantity, uint256 _pricePerToken, address _payToken) external
```

This moethod requires ERC1155 approval from the buyer in prior to the execution

_Buy a digital asset, the asset here is defined by its location in the ledger context.
Payment token here should comply with ERC20 standard
The amount that should be passed into `msg.value` should fulfill the operation and should not be less than _quantity * _pricePerToken_

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| seller | address | The address of the seller |
| ledger | address | The address of the ledger |
| tokenId | uint256 | The id of the token |
| _quantity | uint256 | The quantity of the token |
| _pricePerToken | uint256 | The price per token |
| _payToken | address | The address of the token to pay with, should be ERC20 compliant |

