## OperativeCommon

Base ERC-1155 contract for all operative types in the Elacity DRM ecosystem.
An operative represents the access-control and royalty layer that sits beneath a digital
asset. Three built-in token IDs govern behaviour:
  - `ACCESS_TOKEN` (1) – grants playback / consumption rights.
  - `ROYALTY_SHARE` (2) – entitles the holder to a proportional share of sale revenue.
  - `DISTRIBUTION_RIGHT` (3) – authorises the holder to sell or trade access tokens.

_Upgradeable via beacon proxy. Concrete implementations (`OperativeBuyable`,
`OperativeBuyableSellable`) inherit this contract and define their own `OP_TYPE`._

### ACCESS_TOKEN

```solidity
uint256 ACCESS_TOKEN
```

Token ID representing content-consumption (playback) rights.

### ROYALTY_SHARE

```solidity
uint256 ROYALTY_SHARE
```

Token ID representing royalty shares. Holders receive a pro-rata portion of sale revenue.

### DISTRIBUTION_RIGHT

```solidity
uint256 DISTRIBUTION_RIGHT
```

Token ID that authorises the holder to sell or trade access tokens.
Only addresses with a non-zero balance of this token may transfer `ACCESS_TOKEN`.

### contentId

```solidity
bytes16 contentId
```

Unique identifier linking this operative to its parent digital asset.

### dataStorage

```solidity
contract IStorage dataStorage
```

Reference to the shared ecosystem storage contract.

### royaltyHolders

```solidity
struct EnumerableSet.AddressSet royaltyHolders
```

_Set of addresses that currently hold `ROYALTY_SHARE` tokens._

### accessTransferAuthorized

```solidity
modifier accessTransferAuthorized(address from, uint256[] ids)
```

Reverts with `UnauthorizedTransfer` when `from` lacks the
`DISTRIBUTION_RIGHT` required to transfer access tokens.

### _accessTransferAuthorized

```solidity
function _accessTransferAuthorized(address from, uint256[] ids) internal view returns (bool)
```

_Checks if the caller is authorized to transfer the specified tokens._

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| from | address | The address to check. |
| ids | uint256[] | The token IDs to check. |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | bool | True if the caller is authorized, false otherwise. |

### mintBatchEveryone

```solidity
function mintBatchEveryone(address[] to, uint256[] ids, uint256[] amounts, bytes data) public
```

Mints one token per entry, allowing different recipients in a single call.

_Each index across `to`, `ids`, and `amounts` forms a single mint instruction._

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| to | address[] | Array of recipient addresses. |
| ids | uint256[] | Array of token IDs to mint. |
| amounts | uint256[] | Array of quantities to mint. |
| data | bytes | Optional data forwarded to `_mint`. |

### _checkAccessFor

```solidity
function _checkAccessFor(address account, uint256[] ids) internal view returns (struct IOperative.AccessLevel[])
```

_Checks the access level for the specified account and token IDs._

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| account | address | The address to check. |
| ids | uint256[] | The token IDs to check. |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | struct IOperative.AccessLevel[] | An array of AccessLevel structs. |

### checkAccess

```solidity
function checkAccess(address account) external view virtual returns (struct IOperative.AccessLevel[])
```

Returns the access-level grants for `account` across all relevant token IDs.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| account | address | Address to query. |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | struct IOperative.AccessLevel[] | Array of `AccessLevel` structs indicating which tokens the account holds. |

### uri

```solidity
function uri(uint256 id) public view returns (string)
```

Returns the metadata URI for a specific token ID.

_Appends `/<id>.json` to the base URI set during initialisation._

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| id | uint256 | Token ID to query. |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | string | Fully-qualified metadata URI. |

### royaltyInfo

```solidity
function royaltyInfo(uint256 _salePrice) external view returns (struct IERC2981Enhanced.RoyaltyInfo[])
```

Computes the royalty distribution for a given sale price.

_Each holder's share is proportional to their `ROYALTY_SHARE` balance relative to
the total supply of that token._

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| _salePrice | uint256 | Total sale price to distribute royalties from. |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | struct IERC2981Enhanced.RoyaltyInfo[] | Array of `RoyaltyInfo` structs with each holder's address and owed amount. |

### _update

```solidity
function _update(address from, address to, uint256[] ids, uint256[] values) internal virtual
```

_See {ERC1155-_update}._

### _remapRoyaltyHoldings

```solidity
function _remapRoyaltyHoldings(address from, address to, uint256 amount) internal
```

_Remaps royalty holdings from one address to another. This ensures easy way to retrieve
the list of royalty holders._

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| from | address | The address to remove. |
| to | address | The address to add. |
| amount | uint256 | The amount to transfer. |

### hasTradeAccess

```solidity
function hasTradeAccess(address account, uint256 tkId) public view returns (bool)
```

Determines whether `account` is allowed to trade the token identified by `tkId`.

_For `ROYALTY_SHARE` trades, the caller must hold either an access token or a royalty
share. All other token IDs are permitted by default (subject to `OwnableExclusiveTransferrableTokens`)._

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| account | address | Address to check. |
| tkId | uint256 | Token ID being traded. |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | bool | `true` if the account may trade the specified token. |

### metadataURI

```solidity
function metadataURI() external view returns (string)
```

Returns the contract-level metadata URI, shared with the parent digital asset.

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | string | URI pointing to `contract.json`. |

### supportsInterface

```solidity
function supportsInterface(bytes4 interfaceId) public view virtual returns (bool)
```

### setPaymentProcessor

```solidity
function setPaymentProcessor(address _payProc) external
```

Restrict payment processor changes to the contract owner

### _checkOwnerLater

```solidity
function _checkOwnerLater() internal view
```

Allow acknowledged ecosystem contracts to authorize transfers on operatives

### receive

```solidity
receive() external payable virtual
```

Allows the operative to receive native currency (ETH / ELA) for payment processing.

