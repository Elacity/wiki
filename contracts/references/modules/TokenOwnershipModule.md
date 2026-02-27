## IERCToken

### balanceOf

```solidity
function balanceOf(address) external view returns (uint256)
```

## TokenOwnershipModule

### acceptedTokens

```solidity
struct EnumerableSet.AddressSet acceptedTokens
```

### ownershipThreshold

```solidity
mapping(address => uint256) ownershipThreshold
```

Constant that hold threshold for each token

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |

### configureTokenOwnershipAccess

```solidity
function configureTokenOwnershipAccess(struct ITokenOwnershipAccess.TokenAccessThreshold[] _input) external virtual
```

Configure token-based access.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| _input | struct ITokenOwnershipAccess.TokenAccessThreshold[] | dataset to apply to the contract |

### _configureTokenOwnershipAccess

```solidity
function _configureTokenOwnershipAccess(struct ITokenOwnershipAccess.TokenAccessThreshold[] _input) internal
```

### _registerTokenOwnershipAccess

```solidity
function _registerTokenOwnershipAccess(address tokenAddress, uint256 threshold) internal
```

Add new token with its threshold

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| tokenAddress | address | Token address to add |
| threshold | uint256 | Minimal value of holding |

### _unregisterTokenOwnershipAccess

```solidity
function _unregisterTokenOwnershipAccess(address tokenAddress) internal
```

Remove a token from the contract

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| tokenAddress | address | Token address to remove |

### checkTokenOwnershipAccess

```solidity
function checkTokenOwnershipAccess(address account) public view returns (bool)
```

Check whether an account have access based on its ownership of registered tokens

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| account | address | Address to check access for |

