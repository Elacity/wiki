# IChannelRegistry
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/52ca0e7824ef5fab5ebe0a131f7c6e6dd330de09/contracts/channel/IChannelRegistry.sol)

**Title:**
IChannelRegistry

Defines the wrapper graph and approval workflow between channels.


## Functions
### addWrapper

Requests or finalizes a wrapper relationship for a given channel.

Implementations may finalize immediately when both channels share governance,
otherwise they may place the request into a pending-approval state.


```solidity
function addWrapper(address ch, address wrapper) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`ch`|`address`|Address of the channel to wrap, basically the channel that contains media|
|`wrapper`|`address`|Address of the wrapper, this contract is not supposed to contain media Instead, all medias belonging all channels it wraps should be considered accessible from its context|


### approveWrapper

Approves a previously requested wrapper binding.


```solidity
function approveWrapper(address ch, address wrapper) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`ch`|`address`|Address of the wrappee channel.|
|`wrapper`|`address`|Address of the wrapper channel.|


### topLevelOf

Retrieve the list of all finalized wrappers registered for the given channel.


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
|`<none>`|`address[]`|List of addresses of all the channels that wrap the given one|


### pendingBindingsOf

Retrieve the list of wrapper requests pending approval for a given channel.


```solidity
function pendingBindingsOf(address ch) external view returns (address[] memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`ch`|`address`|Address of the wrappee channel.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address[]`|List of wrapper channels awaiting approval.|


## Events
### ChannelBound
Emitted when a wrapper relationship is finalized between two channels.


```solidity
event ChannelBound(address channel, address wrapper);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`channel`|`address`|This is the wrappee channel, which is at the leaf-side of the tree|
|`wrapper`|`address`|Address of the channel that wraps|

### AwaitBindingApproval
Emitted when a wrapper binding is pending approval from the target channel admin.


```solidity
event AwaitBindingApproval(address channel, address wrapper, address requester);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`channel`|`address`|Wrappee channel awaiting approval.|
|`wrapper`|`address`|Proposed wrapper channel.|
|`requester`|`address`|Acknowledged contract that initiated the binding request.|

