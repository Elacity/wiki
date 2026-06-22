# ChannelRegistry
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/9e5d1dcd32c5761e2bd56d37138c1de7aac83865/contracts/channel/ChannelRegistry.sol)

**Inherits:**
Initializable, [IChannelRegistry](/contracts/channel/IChannelRegistry.md), [ContractIntrospector](/contracts/modules/library/ContractIntrospector.md)

**Title:**
ChannelRegistry

Maintains the parent-child relationship graph between channels, enabling
Multi-Channel subscription propagation.
A **standard channel** is an ERC-1155 contract that hosts digital assets, royalty
distribution, and subscription plans. A **multi-channel** does not host assets itself;
instead it wraps one or more standard channels so that a single subscription grants
access to every wrapped channel.
This registry stores a directed mapping from each wrapped (child) channel to its
wrapping (parent) multi-channels. When a standard channel checks whether an account
has an active subscription (via `ISubscribable.hasActiveSubscription`), it queries
this registry to also consider subscriptions held on any parent multi-channel.

Inherited by `CoreStorage`. The tree is stored as
`mapping(child => EnumerableSet<parent>)`. Wrapper requests are accepted immediately
only when the initiating wrapper and the target channel share a `DEFAULT_ADMIN_ROLE`
holder; otherwise they are queued in `pendingBindings` until a target-channel admin
approves them through `approveWrapper`.


## State Variables
### DEFAULT_ADMIN_ROLE

```solidity
bytes32 private constant DEFAULT_ADMIN_ROLE = 0x00
```


### CHANNEL_REGISTRY_STORAGE_LOCATION
Storage slot for ChannelRegistryStorage.
Formula: keccak256(abi.encode(uint256(keccak256("elacity.drm.storage.ChannelRegistry")) - 1)) & ~bytes32(uint256(0xff))


```solidity
bytes32 private constant CHANNEL_REGISTRY_STORAGE_LOCATION =
    0x5e687b77b333edf260a1ac180b58362202e0da5bc5fa44b89c179b5d6a434800
```


## Functions
### _getChannelRegistryStorage

Retrieves ERC-7201 namespaced storage.


```solidity
function _getChannelRegistryStorage() private pure returns (ChannelRegistryStorage storage $);
```

### addWrapper

Requests or finalizes a wrapper relationship for a given channel.

Restricted to acknowledged contract callers from protocol storage.
Validates both channel endpoints implement `hasRole`, then either finalizes the edge
immediately or records it in `pendingBindings` for later approval.


```solidity
function addWrapper(address ch, address wrapper) external override whitelistOnly(ISystemTracker(address(this)));
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`ch`|`address`|Address of the channel to wrap, basically the channel that contains media|
|`wrapper`|`address`|Address of the wrapper, this contract is not supposed to contain media Instead, all medias belonging all channels it wraps should be considered accessible from its context|


### approveWrapper

Approves a previously requested wrapper binding.


```solidity
function approveWrapper(address ch, address wrapper) external override;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`ch`|`address`|Address of the wrappee channel.|
|`wrapper`|`address`|Address of the wrapper channel.|


### topLevelOf

Retrieve the list of all finalized wrappers registered for the given channel.


```solidity
function topLevelOf(address chan) public view returns (address[] memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`chan`|`address`||

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address[]`|List of addresses of all the channels that wrap the given one|


### pendingBindingsOf

Retrieve the list of wrapper requests pending approval for a given channel.


```solidity
function pendingBindingsOf(address chan) public view returns (address[] memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`chan`|`address`||

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address[]`|List of wrapper channels awaiting approval.|


### _sharesAdminWithInitiator

Determines whether the initiating wrapper flow is controlled by the same admin.

Uses `tx.origin` to compare the external initiator against `DEFAULT_ADMIN_ROLE`
on both the wrappee and wrapper channels.


```solidity
function _sharesAdminWithInitiator(address ch, address wrapper) private view returns (bool);
```

### _requireAccessControlChannel

Ensures the provided channel exposes AccessControl-compatible role checks.


```solidity
function _requireAccessControlChannel(address channel) private view;
```

### _supportsHasRole

Probes whether a channel implements `hasRole(bytes32,address)`.

Used to reject non-channel or non-AccessControl endpoints before registry mutation.


```solidity
function _supportsHasRole(address channel) private view returns (bool supported);
```

## Errors
### InvalidChannel

```solidity
error InvalidChannel(address channel);
```

### MissingBindingApproval

```solidity
error MissingBindingApproval(address channel, address wrapper);
```

### UnauthorizedBindingApproval

```solidity
error UnauthorizedBindingApproval(address channel, address caller);
```

## Structs
### ChannelRegistryStorage
**Note:**
storage-location: erc7201:elacity.drm.storage.ChannelRegistry


```solidity
struct ChannelRegistryStorage {
    /**
     * @dev Structure of the channels tree, wrappers are in leaf side.
     * <target-channel> -> <wrapper-channel>
     */
    mapping(address => EnumerableSet.AddressSet) chTree;
    /**
     * @dev Pending wrapper proposals awaiting approval from the wrappee's admin.
     * <target-channel> -> <wrapper-channel>
     */
    mapping(address => EnumerableSet.AddressSet) pendingBindings;
}
```

