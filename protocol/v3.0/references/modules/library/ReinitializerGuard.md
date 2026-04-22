# ReinitializerGuard
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/52ca0e7824ef5fab5ebe0a131f7c6e6dd330de09/contracts/modules/library/ReinitializerGuard.sol)

**Title:**
ReinitializerGuard

Stateless helper for protecting upgrade reinitializers on transparent proxies.


## State Variables
### PROXY_ADMIN_SLOT

```solidity
bytes32 internal constant PROXY_ADMIN_SLOT = 0xb53127684a568b3173ae13b9f8a6016e243e63b6e8ee1178d6a717850b5d6103
```


## Functions
### _requireAuthorizedReinitializerCaller

Enforces caller authorization after first initialization.


```solidity
function _requireAuthorizedReinitializerCaller(bool alreadyInitialized) internal view;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`alreadyInitialized`|`bool`|True when the proxy has already been initialized at least once.|


### _validateStorageAddress

Ensures the storage dependency matches the expected system-tracker interface.


```solidity
function _validateStorageAddress(address storageAddress) internal view;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`storageAddress`|`address`|Candidate storage contract address.|


### _hasReinitializerRole

Must be implemented by inheriting contracts to check admin-role authorization.


```solidity
function _hasReinitializerRole(address caller) internal view virtual returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`caller`|`address`|Address attempting reinitializer call.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True when caller has an accepted privileged role.|


### _proxyAdmin

Returns the address of the proxy admin.


```solidity
function _proxyAdmin() internal view returns (address);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|The address of the proxy admin.|


### _supportsSystemTracker

Checks if the candidate contract supports the SystemTracker interface.


```solidity
function _supportsSystemTracker(address candidate) private view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`candidate`|`address`|Address of the contract to check.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True when the candidate contract supports the SystemTracker interface.|


## Errors
### UnauthorizedReinitializer
Thrown when initialize/reinitialize is called by an unauthorized sender.


```solidity
error UnauthorizedReinitializer(address caller);
```

### InvalidStorageAddress
Thrown when the provided storage contract is invalid.


```solidity
error InvalidStorageAddress(address storageAddress);
```

