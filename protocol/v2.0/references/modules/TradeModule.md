## IOperativeEnhanced

### owner

```solidity
function owner() external view returns (address)
```

### paymentProcessor

```solidity
function paymentProcessor() external view returns (contract IPaymentProcessor)
```

## ITradeAccessRestriction

This interface defines the requirements for a contract to enable users
interacting with trade gateway contract. For conveniance the contract that implements
it should comply with `ERC-165` as the function is generaly called from outside.

### hasTradeAccess

```solidity
function hasTradeAccess(address account, uint256 tkId) external view returns (bool)
```

Check whether an accunt have access to operate trade

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| account | address | Address of the account to check |
| tkId | uint256 | Target token Id of the tradable token |

## TradeAccessRestriction

This contract is handling the restriction of trade operations

### hasTradeAccess

```solidity
function hasTradeAccess(address _account, uint256 tkId) public view virtual returns (bool)
```

### supportsInterface

```solidity
function supportsInterface(bytes4 interfaceId) public view virtual returns (bool)
```

_See {IERC165-supportsInterface}._

## TradeRestrictionExtension

This extension is handling the restriction of trade operations

```mermaid
 graph LR
     A((Authority Gateway)) --> T{Trade Module}
     T --> |_checkListingAbility| B
     T --> |_restrictTradeOf| C

     classDef red fill:#f5dfdf,stroke:#b86161,stroke-width:2px,color:#b86161;
     classDef green fill:#ebf7df,stroke:#8db861,stroke-width:2px,color:#8db861;
     class B red
     class C green
 ```

### TradableContractFault

```solidity
error TradableContractFault(address contractAddress)
```

Indicates the target contract address is not complying with
the requirements of tradability restriction. The contract should implement
`ERC-165` and `ITradeAccessRestriction`. Eg. inherit it with `TradeAccessRestriction`

### TradeActionRestricted

```solidity
error TradeActionRestricted(address account)
```

Signals an account doesn't have right to trade any of
the tokens on the contract.
Please check how `hasTradeAccess` is implemented, make sure
the `_msgSender()` is allowed to overcome this error.

### restrictTradeOf

```solidity
modifier restrictTradeOf(address contractAddress, uint256 tkId)
```

restrictTradeOf will check if the account has the right to trade
the token

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| contractAddress | address | the address of the contract |
| tkId | uint256 | the token id of the token |

### __TradeRestrictionExtension_init

```solidity
function __TradeRestrictionExtension_init() internal
```

## TradeFoundationModule

This module is handling the foundation of trade operations

```mermaid
 graph LR
     A((Authority Gateway)) --> T{Trade Module}
     T --> |_checkListingAbility| B
     T --> |_sellToken| C
     T --> |_withdrawListing| D

     classDef red fill:#f5dfdf,stroke:#b86161,stroke-width:2px,color:#b86161;
     classDef green fill:#ebf7df,stroke:#8db861,stroke-width:2px,color:#8db861;
     class B red
     class C green
 ```

### __TradeFoundationModule_init

```solidity
function __TradeFoundationModule_init() internal
```

### _checkListingAbility

```solidity
function _checkListingAbility(address owner, address _contractAddr, uint256 tkId, uint256 _quantity) internal view
```

_checkListingAbility will check if the owner has the ability to sell a token

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| owner | address | the address of the owner |
| _contractAddr | address | the address of the contract |
| tkId | uint256 | the token id of the token |
| _quantity | uint256 | the quantity of the token |

### _sellToken

```solidity
function _sellToken(contract IStorage store, address seller, address _opAddr, uint256 tokenId, uint256 _quantity, uint256 _pricePerToken, address _payToken) internal
```

_sellToken will sell a token and place it on sale

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| store | contract IStorage | the storage contract |
| seller | address | the address of the seller |
| _opAddr | address | the address of the operative |
| tokenId | uint256 | the token id of the token |
| _quantity | uint256 | the quantity of the token |
| _pricePerToken | uint256 |  |
| _payToken | address |  |

### _withdrawListing

```solidity
function _withdrawListing(contract IStorage store, address seller, address op, uint256 tkId, uint256 quantity) internal
```

_withdrawListing will withdraw a token from sale

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| store | contract IStorage | the storage contract |
| seller | address | the address of the seller |
| op | address | the address of the operative |
| tkId | uint256 | the token id of the token |
| quantity | uint256 | the quantity of the token |

## AccessTradeModule

This module is handling the workflow of sales and trades of the access token

```mermaid
 graph LR
     B(Buyer) -->|BuyAccess| A((Authority Gateway))
     A --> T{Trade Module}
     T --> |_handlePayout / Amount| C{Royalty Module}
     C -->|Share1| D(fa:fa-pie-chart Seller)
     C -->|Share2| E(fa:fa-pie-chart Receiver #2)
     C -->|Share3| F(fa:fa-pie-chart Receiver #3)
     D -->|ERC1155.safeTransferFrom / Qt=1| B

     classDef red fill:#f5dfdf,stroke:#b86161,stroke-width:2px,color:#b86161;
     classDef green fill:#ebf7df,stroke:#8db861,stroke-width:2px,color:#8db861;
     class B red
     class D green
 ```

### __AccessTradeModule_init

```solidity
function __AccessTradeModule_init() internal
```

### _sellAccess

```solidity
function _sellAccess(contract IStorage store, address seller, address ledger, uint256 tokenId, uint256 _quantity, uint256 _pricePerToken, address _payToken) internal
```

_sellAccess will sell an access token

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| store | contract IStorage | the storage contract |
| seller | address | the address of the seller |
| ledger | address | the address of the ledger |
| tokenId | uint256 | the token id of the token |
| _quantity | uint256 | the quantity of the token |
| _pricePerToken | uint256 |  |
| _payToken | address |  |

### _buyAccess

```solidity
function _buyAccess(contract IStorage store, address seller, address ledger, uint256 tokenId, uint256 _quantity, uint256 _pricePerToken, address _payToken) internal
```

_buyAccess will buy an access token from a seller

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| store | contract IStorage | the storage contract |
| seller | address | the address of the seller |
| ledger | address | the address of the ledger |
| tokenId | uint256 | the token id of the token |
| _quantity | uint256 | the quantity of the token |
| _pricePerToken | uint256 |  |
| _payToken | address |  |

### _handlePayout

```solidity
function _handlePayout(contract IStorage store, address _op, address seller, uint256 _amount, address _payToken) internal
```

_handlePayout will handle the payout of a trade

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| store | contract IStorage | the storage contract |
| _op | address | the address of the operative |
| seller | address | the address of the seller |
| _amount | uint256 | the amount of the trade |
| _payToken | address | the payment token of the trade |

### sellAccess

```solidity
function sellAccess(address ledger, uint256 tokenId, uint256 _quantity, uint256 _pricePerToken, address _payToken) external virtual
```

sellAccess will sell an access token

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| ledger | address | the address of the ledger |
| tokenId | uint256 | the token id of the token |
| _quantity | uint256 | the quantity of the token |
| _pricePerToken | uint256 | the price per token of the token |
| _payToken | address | the payment token of the token |

### sellAccessOnBehalf

```solidity
function sellAccessOnBehalf(address seller, address ledger, uint256 tokenId, uint256 _quantity, uint256 _pricePerToken, address _payToken) external virtual
```

sellAccessOnBehalf will sell an access token on behalf of a seller

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| seller | address | the address of the seller |
| ledger | address | the address of the ledger |
| tokenId | uint256 | the token id of the token |
| _quantity | uint256 | the quantity of the token |
| _pricePerToken | uint256 | the price per token of the token |
| _payToken | address |  |

### withdrawListing

```solidity
function withdrawListing(address op, uint256 tokenId, uint256 quantity) external virtual
```

withdrawListing will withdraw an access token from sale

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| op | address | the address of the operative |
| tokenId | uint256 | the token id of the token |
| quantity | uint256 | the quantity of the token |

### buyAccess

```solidity
function buyAccess(address seller, address ledger, uint256 tokenId, uint256 _quantity, uint256 _pricePerToken) external payable virtual
```

buyAccess will buy an access token from a seller

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| seller | address | the address of the seller |
| ledger | address | the address of the ledger |
| tokenId | uint256 | the token id of the token |
| _quantity | uint256 | the quantity of the token |
| _pricePerToken | uint256 | the price per token of the token |

### buyAccess

```solidity
function buyAccess(address seller, address ledger, uint256 tokenId, uint256 _quantity, uint256 _pricePerToken, address _payToken) external virtual
```

buyAccess will buy an access token from a seller

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| seller | address | the address of the seller |
| ledger | address | the address of the ledger |
| tokenId | uint256 | the token id of the token |
| _quantity | uint256 | the quantity of the token |
| _pricePerToken | uint256 | the price per token of the token |
| _payToken | address | the payment token of the token |

### listings

```solidity
function listings(address op, uint256 tokenId, address seller) external view virtual returns (uint256, uint256, address)
```

Get the listing details of a token

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| op | address | the address of the operative |
| tokenId | uint256 | the token id of the token |
| seller | address | the address of the seller |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | uint256 | the quantity of the token |
| [1] | uint256 | the price per token of the token |
| [2] | address | the payment token of the token |

### sellersOf

```solidity
function sellersOf(address op, uint256 tokenId) external view virtual returns (address[])
```

Get the sellers of a token

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| op | address | the address of the operative |
| tokenId | uint256 | the token id of the token |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | address[] | the addresses of the sellers |

## TokenTradeModule

This module is handling the workflow of sales and trades of the token

```mermaid
 graph LR
     B(Buyer) -->|BuyAccess| A((Authority Gateway))
     A --> T{Trade Module}
     T --> |_handlePayout / Amount| C{Royalty Module}
     T --> |_buyToken / Amount| B
     T --> |_sellToken / Amount| D
     T --> |_putOffer / Amount| E
     T --> |_acceptOffer / Amount| F
     T --> |_cancelOffer / Amount| G

     classDef red fill:#f5dfdf,stroke:#b86161,stroke-width:2px,color:#b86161;
     classDef green fill:#ebf7df,stroke:#8db861,stroke-width:2px,color:#8db861;
     class B red
     class D green
 ```

### __TokenTradeModule_init

```solidity
function __TokenTradeModule_init() internal
```

### _handlePayout

```solidity
function _handlePayout(contract IStorage store, address from, address to, uint256 _amount, address _payToken) internal
```

_handlePayout will handle the payout of a trade

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| store | contract IStorage | the storage contract |
| from | address | the address of the payer |
| to | address | the address of the receiver |
| _amount | uint256 | the amount of the trade |
| _payToken | address | the payment token of the trade |

### withdrawListing

```solidity
function withdrawListing(address op, uint256 tkId, uint256 quantity) external virtual
```

withdrawListing will withdraw a token from sale

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| op | address | the address of the operative |
| tkId | uint256 | the token id of the token |
| quantity | uint256 | the quantity of the token |

### sellToken

```solidity
function sellToken(address _contract, uint256 tokenId, uint256 _quantity, uint256 _pricePerToken, address _payToken) external virtual
```

sellToken will sell a token and place it on sale

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| _contract | address | the contract address of the token |
| tokenId | uint256 | the token id of the token |
| _quantity | uint256 | the quantity of the token |
| _pricePerToken | uint256 | the price per token of the token |
| _payToken | address | the payment token of the token |

### _buyToken

```solidity
function _buyToken(contract IStorage store, address seller, address _contract, uint256 tkId, uint256 _quantity, uint256 _pricePerToken, address _payToken) internal
```

_buyToken will buy a token from a seller

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| store | contract IStorage | the storage contract |
| seller | address | the address of the seller |
| _contract | address | the contract address of the token |
| tkId | uint256 | the token id of the token |
| _quantity | uint256 | the quantity of the token |
| _pricePerToken | uint256 | the price per token of the token |
| _payToken | address | the payment token of the token |

### buyToken

```solidity
function buyToken(address seller, address _contract, uint256 tokenId, uint256 _quantity) external payable virtual
```

buyToken will buy a token from a seller

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| seller | address | the address of the seller |
| _contract | address | the contract address of the token |
| tokenId | uint256 | the token id of the token |
| _quantity | uint256 | the quantity of the token |

### _putOffer

```solidity
function _putOffer(contract IStorage store, address putBy, address _contract, uint256 tokenId, uint256 _quantity, uint256 _pricePerToken, address payToken, bool isAcceptance) internal
```

_putOffer is in charge of settle offer details in the storage

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| store | contract IStorage | the storage contract |
| putBy | address | the address of the offer creator |
| _contract | address | the contract address of the offer |
| tokenId | uint256 | the token id of the offer |
| _quantity | uint256 | the quantity of the offer |
| _pricePerToken | uint256 | the price per token of the offer |
| payToken | address | the payment token of the offer |
| isAcceptance | bool | whether the offer is accepted |

### createOffer

```solidity
function createOffer(address _contract, uint256 tokenId, uint256 _quantity, uint256 _pricePerToken, address payToken) external virtual
```

createOffer creates an offer with ERC-20 token

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| _contract | address | the contract address of the offer |
| tokenId | uint256 | the token id of the offer |
| _quantity | uint256 | the quantity of the offer |
| _pricePerToken | uint256 | the price per token of the offer |
| payToken | address |  |

### createOffer

```solidity
function createOffer(address _contract, uint256 tokenId, uint256 _quantity, uint256 _pricePerToken) external payable virtual
```

createOffer creates an offer with native token (ETH)

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| _contract | address | the contract address of the offer |
| tokenId | uint256 | the token id of the offer |
| _quantity | uint256 | the quantity of the offer |
| _pricePerToken | uint256 | the price per token of the offer |

### _acceptOffer

```solidity
function _acceptOffer(contract IStorage store, address offerBy, address _contract, uint256 tokenId, uint256 _quantity) internal
```

_acceptOffer will accept an offer made by a buyer

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| store | contract IStorage | the storage contract |
| offerBy | address | the address of the offer creator |
| _contract | address | the contract address of the offer |
| tokenId | uint256 | the token id of the offer |
| _quantity | uint256 | the quantity of the offer |

### acceptOffer

```solidity
function acceptOffer(address from, address _contract, uint256 tokenId, uint256 _quantity) external virtual
```

acceptOffer will accept an offer made by a buyer

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| from | address | the address of the offer creator |
| _contract | address | the contract address of the offer |
| tokenId | uint256 | the token id of the offer |
| _quantity | uint256 | the quantity of the offer |

### cancelOffer

```solidity
function cancelOffer(address _contract, uint256 tokenId) external virtual
```

cancelOffer will withdraw an offer made by its creator

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| _contract | address | the contract address of the offer |
| tokenId | uint256 | the token id of the offer |

