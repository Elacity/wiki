# TokenOwnershipModule
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/52ca0e7824ef5fab5ebe0a131f7c6e6dd330de09/contracts/modules/access-control/TokenOwnershipModule.sol)

**Inherits:**
[ITokenOwnershipAccess](/contracts/modules/access-control/ITokenOwnershipAccess.md)

**Title:**
TokenOwnershipModule

Abstract contract that implements the `ITokenOwnershipAccess` interface


## State Variables
### TOKEN_OWNERSHIP_MODULE_STORAGE_LOCATION

```solidity
bytes32 private constant TOKEN_OWNERSHIP_MODULE_STORAGE_LOCATION =
    0x27436ca4b78f793583f35c57d8ff91dc6c9e3f5316b5041c5e965294093d6a00
```


## Functions
### _getTokenOwnershipModuleStorage


```solidity
function _getTokenOwnershipModuleStorage() private pure returns (TokenOwnershipModuleStorage storage $);
```

### ownershipThreshold

Get the ownership threshold for a token.


```solidity
function ownershipThreshold(address tokenAddress) public view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenAddress`|`address`|Token address to get the ownership threshold for|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The ownership threshold for the token|


### configureTokenOwnershipAccess

Configure token ownership access


```solidity
function configureTokenOwnershipAccess(TokenAccessThreshold[] calldata _input) external virtual;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_input`|`TokenAccessThreshold[]`|Array of token access thresholds|


### _configureTokenOwnershipAccess

Internal function to configure token ownership access


```solidity
function _configureTokenOwnershipAccess(TokenAccessThreshold[] calldata _input) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_input`|`TokenAccessThreshold[]`|Array of token access thresholds|


### _registerTokenOwnershipAccess

Add new token with its threshold


```solidity
function _registerTokenOwnershipAccess(address tokenAddress, uint256 threshold) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenAddress`|`address`|Token address to add|
|`threshold`|`uint256`|Minimal value of holding|


### _unregisterTokenOwnershipAccess

Remove a token from the contract


```solidity
function _unregisterTokenOwnershipAccess(address tokenAddress) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenAddress`|`address`|Token address to remove|


### _setTokenOwnershipAccess

Sets token ownership access state without emitting.

Intended for initialization flows where EventHub emission must be deferred.


```solidity
function _setTokenOwnershipAccess(address tokenAddress, uint256 threshold) internal;
```

### _clearTokenOwnershipAccess

Clears token ownership access state without emitting.

Intended for initialization flows where EventHub emission must be deferred.


```solidity
function _clearTokenOwnershipAccess(address tokenAddress) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenAddress`|`address`|Token address to clear|


### _resolveEventHub

Resolves the EventHub used for token-access events.

Default path resolves from the inheriting provider's `cstore()` reference.


```solidity
function _resolveEventHub() internal view virtual returns (IEventHub);
```

### checkTokenOwnershipAccess

Check if an account has access to the contract


```solidity
function checkTokenOwnershipAccess(address account) public view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`account`|`address`|Account to check|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if the account has access, false otherwise|


## Structs
### TokenOwnershipModuleStorage
**Note:**
storage-location: erc7201:elacity.drm.storage.TokenOwnershipModule


```solidity
struct TokenOwnershipModuleStorage {
    EnumerableSet.AddressSet acceptedTokens;
    mapping(address => uint256) ownershipThreshold;
}
```

