# ChannelConfigurable
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/674fb60a18e2aa14b7080f0f43e11002723bd5b3/contracts/channel/ChannelConfigurable.sol)

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


