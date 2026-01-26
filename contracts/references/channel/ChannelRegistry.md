## ChannelRegistry

This contract is aimed to hold how channels are interacting with each other
        especially in regards of Multi-Channels.

A channel is a ERC-1155 compliant contract and contains all medias, royalty distribution
and all subscription plans attached to the channel.

For the case of multi-channels, it doesn't contains any media, it rather hold a list of
all channels that it wraps. A user that is subscribed to a multi-channels should have
access into all channels wrapped in it.

This contract will hold such a structure and ensure we can give the access abstraction
to all subscribers from individual channel interface.

**How it is organized?**
- Each channel contract, regardless of its type, should have this registry embed on it
- We input channels configuration from all MultiChannel contract and configure the
  registry so that each wrapped contracts can point into the wrapping channel during a
  lookup method execution
- on lookup execution from individual channel (in compliance with `ISubscribable` interface)
  we should be able to get all mulit-channel contract that are wrapping the target channel

### __ChannelRegistry_init

```solidity
function __ChannelRegistry_init() internal
```

_Internal function to initialize the contract
This is called by the root contract (CoreStorage) during its initialization_

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
function topLevelOf(address chan) public view returns (address[])
```

