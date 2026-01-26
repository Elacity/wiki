## MultiChannelFactory

### constructor

```solidity
constructor(address _authority, address _tradeGateway, address __store, contract IPaymentProcessorFactory _ppf, address _implementation) public
```

### createChannel

```solidity
function createChannel(address creator, string _name, string _tokenURI, bytes _data) external payable returns (address)
```

Create a new channel contract

The kind of the target channel is defined in the factory implementation

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| creator | address | Address of the creator/owner, basically it's the initiator of the transaction |
| _name | string | The name of the channel |
| _tokenURI | string |  |
| _data | bytes | Initialization and configuration data |

