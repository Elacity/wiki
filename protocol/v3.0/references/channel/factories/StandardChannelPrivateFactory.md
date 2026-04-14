# StandardChannelPrivateFactory
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/a429f79c38ae4f5221da86eca62d9868f0a5a7fd/contracts/channel/factories/StandardChannelFactory.sol)

**Inherits:**
[StandardChannelFactory](/contracts/channel/factories/StandardChannelFactory.md)

**Title:**
StandardChannelPrivateFactory

Deploys `DigitalAssetPrivate` channels where only the `MINTER_ROLE` holder can mint.


## Functions
### constructor

**Note:**
docs-ignore: true


```solidity
constructor(
    address _authority,
    address _tradeGateway,
    address _registry,
    address _subscriptionManager,
    IPaymentProcessorFactory _ppf,
    address _implementation
) StandardChannelFactory(_authority, _tradeGateway, _registry, _subscriptionManager, _ppf, _implementation);
```

### createChannel

Create a new channel contract
The kind of the target channel is defined in the factory implementation


```solidity
function createChannel(address creator, string memory _name, string memory _tokenURI, bytes calldata _data)
    external
    payable
    override
    collectChannelCreationFee
    returns (address);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`creator`|`address`|Address of the creator/owner, basically it's the initiator of the transaction|
|`_name`|`string`|The name of the channel|
|`_tokenURI`|`string`||
|`_data`|`bytes`|Initialization and configuration data|


