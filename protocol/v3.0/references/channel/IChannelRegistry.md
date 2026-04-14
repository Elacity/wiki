# IChannelRegistry
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/a429f79c38ae4f5221da86eca62d9868f0a5a7fd/contracts/channel/IChannelRegistry.sol)

**Title:**
IChannelRegistry

Defines a channel registry contract


## Functions
### addWrapper

Add new wrapper for a given channel. Basically, the wrapper is a multi-channel
contract and the wrappee can be either a multi-channel or a digital assets channel
that contains the medias


```solidity
function addWrapper(address ch, address wrapper) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`ch`|`address`|Address of the channel to wrap, basically the channel that contains media|
|`wrapper`|`address`|Address of the wrapper, this contract is not supposed to contain media Instead, all medias belonging all channels it wraps should be considered accessible from its context|


### topLevelOf

Retrieve the list of all wrapper on top-level f the given channel


```solidity
function topLevelOf(address ch) external view returns (address[] memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`ch`|`address`|Address of the wrappee channel|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address[]`|List of addresse of all the channels that wrap the given one|


## Events
### ChannelBound
This event is trigger when 2 channels are hooked to each other


```solidity
event ChannelBound(address channel, address wrapper);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`channel`|`address`|This is the wrappee channel, which is at the leaf-side of the tree|
|`wrapper`|`address`|Address of the channel that wraps|

