# ChannelConfigurable
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/a429f79c38ae4f5221da86eca62d9868f0a5a7fd/contracts/channel/ChannelConfigurable.sol)

**Title:**
ChannelConfigurable

Defines the ability to operate a configuration statement onto a channel
This contract has been set as `abstract` so that each channel type have their
own implementation in regards of how the channel is initialized.


## Functions
### configureChannel

Operates the configuration and initialization of the channel


```solidity
function configureChannel(bytes memory data) internal virtual;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`data`|`bytes`|Raw data to process the configuration with|


