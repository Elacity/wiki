# StandardChannelPublicFactory
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/674fb60a18e2aa14b7080f0f43e11002723bd5b3/contracts/channel/factories/StandardChannelFactory.sol)

**Inherits:**
[StandardChannelFactory](../../channel/factories/StandardChannelFactory.md)

**Title:**
StandardChannelPublicFactory

Deploys `DigitalAssetPublic` channels where any user can mint content.


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


