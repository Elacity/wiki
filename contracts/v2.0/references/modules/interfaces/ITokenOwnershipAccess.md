## ITokenOwnershipAccess

`ITokenOwnershipAccess` interface defines the way to manage access based on
token (ERC20 or ERC721) ownership.

To enable flexibility, all contracts that supports `balanceOf` function are intended to
be supported.

### TokenAccessRegistered

```solidity
event TokenAccessRegistered(address _tokenAddress, uint256 threshold)
```

Event triggered when a new token is registered to the contract implementation

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| _tokenAddress | address | Address of the token to consider (ERC20, ERC721) |
| threshold | uint256 | Amount minimal to enable the access |

### TokenAccessRemoved

```solidity
event TokenAccessRemoved(address _tokenAddress)
```

Event triggered when a token have been withdrawn from the token-based access system

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| _tokenAddress | address | Address of the token that have been removed |

### TokenAccessThreshold

This struct represents a set of settings that define the Token-based access

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |

```solidity
struct TokenAccessThreshold {
  address tokenAddress;
  uint256 threshold;
}
```

### ownershipThreshold

```solidity
function ownershipThreshold(address tokenAddress) external view returns (uint256)
```

Constant that hold threshold for each token

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| tokenAddress | address | Address of the token |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | uint256 | Value of the threshold in `uint256` |

### configureTokenOwnershipAccess

```solidity
function configureTokenOwnershipAccess(struct ITokenOwnershipAccess.TokenAccessThreshold[] _input) external
```

Configure token-based access.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| _input | struct ITokenOwnershipAccess.TokenAccessThreshold[] | dataset to apply to the contract |

### checkTokenOwnershipAccess

```solidity
function checkTokenOwnershipAccess(address account) external view returns (bool)
```

Check whether an account have access based on its ownership of registered tokens

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| account | address | Address to check access for |

