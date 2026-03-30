## IChannelRegistry

Defines a channel registry contract

### ChannelBound

```solidity
event ChannelBound(address channel, address wrapper)
```

This event is trigger when 2 channels are hooked to each other

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| channel | address | This is the wrappee channel, which is at the leaf-side of the tree |
| wrapper | address | Address of the channel that wraps |

### addWrapper

```solidity
function addWrapper(address ch, address wrapper) external
```

Add new wrapper for a given channel. Basically, the wrapper is a multi-channel
contract and the wrappee can be either a multi-channel or a digital assets channel
that contains the medias

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| ch | address | Address of the channel to wrap, basically the channel that contains media |
| wrapper | address | Address of the wrapper, this contract is not supposed to contain media Instead, all medias belonging all channels it wraps should be considered accessible from its context |

### topLevelOf

```solidity
function topLevelOf(address ch) external view returns (address[])
```

Retrieve the list of all wrapper on top-level f the given channel

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| ch | address | Address of the wrappee channel |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | address[] | List of addresse of all the channels that wrap the given one |

