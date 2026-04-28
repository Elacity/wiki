## ChannelConfigurable

Defines the ability to operate a configuration statement onto a channel

This contract has been set as `abstract` so that each channel type have their
own implementation in regards of how the channel is initialized.

### configureChannel

```solidity
function configureChannel(bytes data) internal virtual
```

Operates the configuration and initialization of the channel

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| data | bytes | Raw data to process the configuration with |

