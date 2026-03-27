## AuthorityGateway

This is the main front contract that governs the access to medias
It allows to sell, and buy these access tokens, also this is the main contract
that allows to acquire a license for a specific media.

_About versionning and the `reinitializer(uint64)` modifier:
It is a croissant number and contains 8-bytes to comply with
`reinitializer(uint64)` modifier of the [`Initializable` contract](https://github.com/OpenZeppelin/openzeppelin-contracts-upgradeable/blob/2c1de3d1a6689233a0469375cb51a41f4ad9ec05/contracts/proxy/utils/Initializable.sol#L152C14-L152C27).

*How it is formed?*
 - version of Authority gateway: eg. 2.0 -> [0x02, 0x00]
 - deployment version eg: (ecosystem iteration) 0.6.0: [0x00, 0x06, 0x00]
 - we don't use 3-bytes first bytes and reserve it for future and ensure it keeps increasing_

### StorageFault

```solidity
error StorageFault(address s)
```

StorageFault error

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| s | address | Address of the storage contract |

### UnboundContentId

```solidity
error UnboundContentId(bytes16 contentId)
```

UnboundContentId error

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| contentId | bytes16 | Content ID of the media |

### dataStorage

```solidity
contract IStorage dataStorage
```

Data storage contract

### constructor

```solidity
constructor() public
```

### _hasReinitializerRole

```solidity
function _hasReinitializerRole(address caller) internal view returns (bool)
```

Must be implemented by inheriting contracts to check admin-role authorization.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| caller | address | Address attempting reinitializer call. |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | bool | True when caller has an accepted privileged role. |

### sellAccess

```solidity
function sellAccess(address ledger, uint256 tokenId, uint256 _quantity, uint256 _pricePerToken, address _payToken) external
```

Sell access tokens

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| ledger | address | Address of the ledger contract |
| tokenId | uint256 | Token ID of the access token |
| _quantity | uint256 | Quantity of access tokens to sell |
| _pricePerToken | uint256 | Price per access token |
| _payToken | address | Address of the token to be paid |

### sellAccessOnBehalf

```solidity
function sellAccessOnBehalf(address seller, address ledger, uint256 tokenId, uint256 _quantity, uint256 _pricePerToken, address _payToken) external
```

Sell access tokens, it differs from `.sellAccess` method from its execution context.
Only an acknowledged contract can call this method.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| seller | address | Address of the seller |
| ledger | address | Address of the ledger contract |
| tokenId | uint256 | Token ID of the access token |
| _quantity | uint256 | Quantity of access tokens to sell |
| _pricePerToken | uint256 | Price per access token |
| _payToken | address | Address of the token to be paid |

### buyAccess

```solidity
function buyAccess(address seller, address ledger, uint256 tokenId, uint256 _quantity, uint256 _pricePerToken) external payable
```

Buy access tokens

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| seller | address | Address of the seller |
| ledger | address | Address of the ledger contract |
| tokenId | uint256 | Token ID of the access token |
| _quantity | uint256 | Quantity of access tokens to buy |
| _pricePerToken | uint256 | Price per access token |

### buyAccess

```solidity
function buyAccess(address seller, address ledger, uint256 tokenId, uint256 _quantity, uint256 _pricePerToken, address _payToken) external
```

Buy access tokens

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| seller | address | Address of the seller |
| ledger | address | Address of the ledger contract |
| tokenId | uint256 | Token ID of the access token |
| _quantity | uint256 | Quantity of access tokens to buy |
| _pricePerToken | uint256 | Price per access token |
| _payToken | address | Address of the token to be paid |

### withdrawListing

```solidity
function withdrawListing(address op, uint256 tokenId, uint256 quantity) external
```

Withdraw listing from the marketplace

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| op | address | Address of the operative contract |
| tokenId | uint256 | Token ID of the access token |
| quantity | uint256 | Quantity of access tokens to withdraw |

### acquireLicense

```solidity
function acquireLicense(bytes req) external view returns (bytes4, bytes)
```

Acquire a license for a given Digital Asset

_Since `0.8.0`, this method is put on deprecation phase in profit of usage of Lit Protocol Key Management_

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

Get the operative contract for a given ledger and token ID

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| ledger | address | Address of the ledger contract |
| tokenId | uint256 | Token ID of the access token |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | address | _op Address of the operative contract |
| [1] | contract IOperative | IOperative(_op) Instance of the operative contract |

### operative

```solidity
function operative(address ledger, uint256 tokenId) external view returns (address)
```

Get the operative contract for a given ledger and token ID

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| ledger | address | Address of the ledger contract |
| tokenId | uint256 | Token ID of the access token |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | address | _op Address of the operative contract |

### sellersOf

```solidity
function sellersOf(address op, uint256 tokenId) external view returns (address[])
```

Get the sellers of a given operative contract and token ID

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| op | address | Address of the operative contract |
| tokenId | uint256 | Token ID of the access token |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | address[] | sellers Array of sellers |

### listings

```solidity
function listings(address op, uint256 tokenId, address seller) external view returns (uint256, uint256, address)
```

Get the listing for a given operative contract and token ID

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| op | address | Address of the operative contract |
| tokenId | uint256 | Token ID of the access token |
| seller | address | Address of the seller |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | uint256 | quantity Quantity of access tokens |
| [1] | uint256 | pricePerToken Price per access token |
| [2] | address | payToken Address of the token to be paid |

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

