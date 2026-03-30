# ChannelRegistry
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/674fb60a18e2aa14b7080f0f43e11002723bd5b3/contracts/channel/ChannelRegistry.sol)

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
`mapping(child => EnumerableSet<parent>)` and is append-only in the current design.


## State Variables
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

Add new wrapper for a given channel. Basically, the wrapper is a multi-channel
contract and the wrappee can be either a multi-channel or a digital assets channel
that contains the medias

Restricted to acknowledged contract callers from protocol storage.


```solidity
function addWrapper(address ch, address wrapper) external override whitelistOnly(ISystemTracker(address(this)));
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`ch`|`address`|Address of the channel to wrap, basically the channel that contains media|
|`wrapper`|`address`|Address of the wrapper, this contract is not supposed to contain media Instead, all medias belonging all channels it wraps should be considered accessible from its context|


### topLevelOf

Retrieve the list of all wrapper on top-level f the given channel


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
|`<none>`|`address[]`|List of addresse of all the channels that wrap the given one|


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
}
```

