# EventHubResolver
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/a429f79c38ae4f5221da86eca62d9868f0a5a7fd/contracts/events/EventHubResolver.sol)

**Title:**
EventHubResolver

Helpers to resolve the EventHub from storage-anchored protocol contracts.


## State Variables
### SLOT_EVENT_HUB
Registry slot key for the EventHub address.


```solidity
bytes32 internal constant SLOT_EVENT_HUB = keccak256("slot.eventHub")
```


### SELECTOR_CONTRACT_AT
Selector for `contractAt(bytes32)`.


```solidity
bytes4 private constant SELECTOR_CONTRACT_AT = ISystemTracker.contractAt.selector
```


### SELECTOR_CSTORE
Selector for `cstore()`.


```solidity
bytes4 private constant SELECTOR_CSTORE = bytes4(keccak256("cstore()"))
```


## Functions
### tryResolveFromStorage

Attempts to resolve EventHub directly from storage address.


```solidity
function tryResolveFromStorage(address storageAddress) internal view returns (IEventHub hub);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`storageAddress`|`address`|Address expected to implement `contractAt(bytes32)`.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`hub`|`IEventHub`|Event hub interface or zero-address interface when unavailable.|


### resolveFromStorage

Resolves EventHub directly from storage address.

Reverts when EventHub is unavailable or unresolved.


```solidity
function resolveFromStorage(address storageAddress) internal view returns (IEventHub hub);
```

### tryResolveFromProvider

Attempts to resolve EventHub from a provider contract exposing `cstore()`.


```solidity
function tryResolveFromProvider(address provider) internal view returns (IEventHub hub);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`provider`|`address`|Address of the contract that exposes a `cstore()` storage reference getter.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`hub`|`IEventHub`|Event hub interface or zero-address interface when unavailable.|


### resolveFromProvider

Resolves EventHub from a provider exposing `cstore()`.

Reverts when EventHub is unavailable or unresolved.


```solidity
function resolveFromProvider(address provider) internal view returns (IEventHub hub);
```

## Errors
### EventHubUnavailable
EventHub cannot be resolved from storage/provider.


```solidity
error EventHubUnavailable(address source);
```

