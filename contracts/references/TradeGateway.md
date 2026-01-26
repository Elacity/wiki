## TradeGateway

It is the entry point for all trade operations

### store

```solidity
contract IStorage store
```

### constructor

```solidity
constructor() public
```

### initialize

```solidity
function initialize(contract IStorage _dataStorage) public
```

### sellersOf

```solidity
function sellersOf(address op, uint256 tkId) external view returns (address[])
```

Get the sellers of a token

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| op | address | the address of the operative |
| tkId | uint256 | the token id of the token |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | address[] | the addresses of the sellers |

### listings

```solidity
function listings(address op, uint256 tkId, address seller) external view returns (uint256, uint256, address)
```

Get the listing details of a token

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| op | address | the address of the operative |
| tkId | uint256 | the token id of the token |
| seller | address | the address of the seller |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | uint256 | the quantity of the token |
| [1] | uint256 | the price per token of the token |
| [2] | address | the payment token of the token |

### withdrawListing

```solidity
function withdrawListing(address op, uint256 tkId, uint256 quantity) external
```

withdrawListing withdraws a token from sale

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| op | address | the address of the operative |
| tkId | uint256 | the token id of the token |
| quantity | uint256 | the quantity of the token |

### sellToken

```solidity
function sellToken(address _contract, uint256 tokenId, uint256 _quantity, uint256 _pricePerToken, address _payToken) external
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

### buyToken

```solidity
function buyToken(address seller, address _contract, uint256 tokenId, uint256 _quantity) external payable
```

buyToken will buy a token from a seller

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| seller | address | the address of the seller |
| _contract | address | the contract address of the token |
| tokenId | uint256 | the token id of the token |
| _quantity | uint256 | the quantity of the token |

### createOffer

```solidity
function createOffer(address _contract, uint256 tokenId, uint256 _quantity, uint256 _pricePerToken, address payToken) external
```

createOffer creates an offer with ERC-20 token

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| _contract | address | the contract address of the offer |
| tokenId | uint256 | the token id of the offer |
| _quantity | uint256 | the quantity of the offer |
| _pricePerToken | uint256 | the price per token of the offer |
| payToken | address | the payment token of the offer |

### createOffer

```solidity
function createOffer(address _contract, uint256 tokenId, uint256 _quantity, uint256 _pricePerToken) external payable
```

createOffer creates an offer with native token (ETH)

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| _contract | address | the contract address of the offer |
| tokenId | uint256 | the token id of the offer |
| _quantity | uint256 | the quantity of the offer |
| _pricePerToken | uint256 | the price per token of the offer |

### acceptOffer

```solidity
function acceptOffer(address from, address _contract, uint256 tokenId, uint256 _quantity) external
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
function cancelOffer(address _contract, uint256 tokenId) external
```

cancelOffer will withdraw an offer made by its creator

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| _contract | address | the contract address of the offer |
| tokenId | uint256 | the token id of the offer |

