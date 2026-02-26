## OperativeBuyableSellable

"Buy, play, and resell" operative (type 2).
Extends the buy-play model by allowing access-token holders to resell their tokens
on the secondary market. When an access token is transferred, distribution rights
are automatically granted to the new owner and revoked from the previous one
(if they no longer hold any access tokens).

_Deployed behind a beacon proxy via `OperativeBuyableSellableFactory`.
The `resellerCut` (basis-point percentage, max 950 = 95 %) determines how much
of the resale price the reseller retains._

### UnauthorizedDistrbutorError

```solidity
error UnauthorizedDistrbutorError(address from)
```

Thrown when `from` does not hold a `DISTRIBUTION_RIGHT` token.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| from | address | Address that attempted the unauthorised action. |

### ResellerCutOverflow

```solidity
error ResellerCutOverflow(uint256 value)
```

Thrown when the requested reseller cut exceeds the protocol cap (950 basis points).

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| value | uint256 | The invalid cut value that was supplied. |

### OP_TYPE

```solidity
uint16 OP_TYPE
```

Operative-type discriminator (`2` = buy-play-sell).

### resellerCut

```solidity
uint16 resellerCut
```

Percentage of resale price retained by the reseller, in basis points (max 950).

### onlyDistributor

```solidity
modifier onlyDistributor(address from)
```

Restricts the decorated function to holders of `DISTRIBUTION_RIGHT` tokens.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| from | address | Address to verify. |

### setupDistributionRights

```solidity
function setupDistributionRights(address creator) external
```

Mints a single `DISTRIBUTION_RIGHT` token to the content creator.

_Must be called by the factory immediately after proxy initialisation._

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| creator | address | Address that will receive the distribution-right token. |

### _isDistributor

```solidity
function _isDistributor(address from) internal view returns (bool)
```

### setResellerCut

```solidity
function setResellerCut(uint16 _resellerCut) public
```

Updates the reseller cut percentage.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| _resellerCut | uint16 | New cut in basis points (0–950, where 950 = 95 %). |

### checkAccess

```solidity
function checkAccess(address account) external view returns (struct IOperative.AccessLevel[])
```

Returns the access levels for `account`, checking `ACCESS_TOKEN` and `DISTRIBUTION_RIGHT`.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| account | address | Address to query. |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | struct IOperative.AccessLevel[] | Array of `AccessLevel` structs for each checked token. |

### _update

```solidity
function _update(address from, address to, uint256[] ids, uint256[] values) internal virtual
```

_See {ERC1155-_update}._

### supportsInterface

```solidity
function supportsInterface(bytes4 interfaceId) public view virtual returns (bool)
```
