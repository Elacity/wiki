## ChannelRegistry

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

_Inherited by `CoreStorage`. The tree is stored as
`mapping(child => EnumerableSet<parent>)` and is append-only in the current design._

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

Retrieve the list of all wrapper on top-level f the given channel

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| chan | address |  |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | address[] | List of addresse of all the channels that wrap the given one |

