## OperativeBuyable

"Buy once, play always" operative (type 1).
Purchasers receive an `ACCESS_TOKEN` that grants permanent playback rights.
Only the original creator (or a designated address) holds a `DISTRIBUTION_RIGHT`
token, making them the sole party authorised to sell new access tokens.

_Deployed behind a beacon proxy via `OperativeBuyableFactory`._

### OP_TYPE

```solidity
uint16 OP_TYPE
```

Operative-type discriminator (`1` = buy-play).

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

