## AuthorityGateway

### StorageFault

```solidity
error StorageFault(address s)
```

### UnboundContentId

```solidity
error UnboundContentId(bytes16 contentId)
```

### dataStorage

```solidity
contract IStorage dataStorage
```

### constructor

```solidity
constructor() public
```

### initialize

```solidity
function initialize(contract IStorage _dataStorage) public
```

### sellAccess

```solidity
function sellAccess(address ledger, uint256 tokenId, uint256 _quantity, uint256 _pricePerToken, address _payToken) external
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
function sellAccessOnBehalf(address seller, address ledger, uint256 tokenId, uint256 _quantity, uint256 _pricePerToken, address _payToken) external
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

### buyAccess

```solidity
function buyAccess(address seller, address ledger, uint256 tokenId, uint256 _quantity, uint256 _pricePerToken) external payable
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
function buyAccess(address seller, address ledger, uint256 tokenId, uint256 _quantity, uint256 _pricePerToken, address _payToken) external
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

### withdrawListing

```solidity
function withdrawListing(address op, uint256 tokenId, uint256 quantity) external
```

withdrawListing will withdraw an access token from sale

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| op | address | the address of the operative |
| tokenId | uint256 | the token id of the token |
| quantity | uint256 | the quantity of the token |

### acquireLicense

```solidity
function acquireLicense(bytes req) external view returns (bytes4, bytes)
```

Acquire a license for a given Digital Asset

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| req | bytes | The request data |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | bytes4 | The result of the license acquisition |
| [1] | bytes | The license data |

### hasAccess

```solidity
function hasAccess(address accessor, address ledger, uint256 tokenId) external view returns (bool)
```

Check if the caller has access to the given token

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| accessor | address | The accessor address |
| ledger | address | The ledger address |
| tokenId | uint256 | The token ID |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | bool | hasAccess True if the caller has access, false otherwise |

### hasAccessByContentId

```solidity
function hasAccessByContentId(address accessor, bytes16 contentId) external view returns (bool)
```

Check whether an accessor address has access to a media referenced by its KID

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| accessor | address | The accessor address |
| contentId | bytes16 | The content ID of the media |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | bool | hasAccess True if the accessor has access, false otherwise |

### _getOperative

```solidity
function _getOperative(address ledger, uint256 tokenId) internal view returns (address, contract IOperative)
```

### operative

```solidity
function operative(address ledger, uint256 tokenId) external view returns (address)
```

### sellersOf

```solidity
function sellersOf(address op, uint256 tokenId) external view returns (address[])
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

### listings

```solidity
function listings(address op, uint256 tokenId, address seller) external view returns (uint256, uint256, address)
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

### supportsLitProtocol

```solidity
function supportsLitProtocol() external pure returns (bool)
```

Check if the Authority Gateway supports the Lit Protocol
CEK bindings. Only recent versions should have it.
This function will serve as a way to check if the Authority Gateway
supports the Lit Protocol from contract client side.

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | bool | True if the Authority Gateway supports the Lit Protocol, false otherwise |

