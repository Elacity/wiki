## ReinitializerGuard

Stateless helper for protecting upgrade reinitializers on transparent proxies.

### PROXY_ADMIN_SLOT

```solidity
bytes32 PROXY_ADMIN_SLOT
```

### UnauthorizedReinitializer

```solidity
error UnauthorizedReinitializer(address caller)
```

Thrown when initialize/reinitialize is called by an unauthorized sender.

### InvalidStorageAddress

```solidity
error InvalidStorageAddress(address storageAddress)
```

Thrown when the provided storage contract is invalid.

### _requireAuthorizedReinitializerCaller

```solidity
function _requireAuthorizedReinitializerCaller(bool alreadyInitialized) internal view
```

Enforces caller authorization after first initialization.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| alreadyInitialized | bool | True when the proxy has already been initialized at least once. |

### _validateStorageAddress

```solidity
function _validateStorageAddress(address storageAddress) internal view
```

Ensures the storage dependency is a deployed contract.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| storageAddress | address | Candidate storage contract address. |

### _hasReinitializerRole

```solidity
function _hasReinitializerRole(address caller) internal view virtual returns (bool)
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

### _proxyAdmin

```solidity
function _proxyAdmin() internal view returns (address)
```

