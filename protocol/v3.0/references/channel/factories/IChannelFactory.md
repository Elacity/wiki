# IChannelFactory
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/9e5d1dcd32c5761e2bd56d37138c1de7aac83865/contracts/channel/factories/IChannelFactory.sol)

**Title:**
IChannelFactory

This interface provides a channel factory minimal behavior


## Functions
### createChannel

Create a new channel contract
The kind of the target channel is defined in the factory implementation


```solidity
function createChannel(address creator, string memory _name, string memory _tokenUri, bytes calldata _data)
    external
    payable
    returns (address);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`creator`|`address`|Address of the creator/owner, basically it's the initiator of the transaction|
|`_name`|`string`|The name of the channel|
|`_tokenUri`|`string`|Base metadata URI for the channel contract|
|`_data`|`bytes`|Initialization and configuration data|


