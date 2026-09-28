# ChannelConfigurable
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/bc1f2ea3fd5d8b703627a7946e7d5fe7fb13f047/contracts/channel/ChannelConfigurable.sol)

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


