## StandardChannelFactory

Abstract factory for deploying standard (asset-hosting) channels behind a beacon proxy.
Concrete subclasses (`StandardChannelPublicFactory`, `StandardChannelPrivateFactory`)
select the appropriate channel implementation at deployment time.

### authority

```solidity
address authority
```

### createChannel

```solidity
function createChannel(address creator, string _name, string _tokenURI, bytes _data) external payable virtual returns (address)
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

## StandardChannelPublicFactory

Deploys `DigitalAssetPublic` channels where any user can mint content.

### createChannel

```solidity
function createChannel(address creator, string _name, string _tokenURI, bytes _data) external payable returns (address)
```

## StandardChannelPrivateFactory

Deploys `DigitalAssetPrivate` channels where only the `MINTER_ROLE` holder can mint.

### createChannel

```solidity
function createChannel(address creator, string _name, string _tokenURI, bytes _data) external payable returns (address)
```

