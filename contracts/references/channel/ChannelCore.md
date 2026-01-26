## ChannelCore

### CreateFailed

```solidity
error CreateFailed()
```

### FactoryUnknownError

```solidity
error FactoryUnknownError(bytes)
```

### FactoryNotFound

```solidity
error FactoryNotFound(uint8 _channelType, uint8 _scope)
```

### ChannelCreated

```solidity
event ChannelCreated(address channelAddr, uint8 channelType, address owner, uint8 scope, string name)
```

This event is triggered when a new channel contract have been created

### constructor

```solidity
constructor() public
```

### initialize

```solidity
function initialize() public
```

### registerFactory

```solidity
function registerFactory(uint8 _channelType, uint8 _scope, address factoryAddr) external
```

Register a new factory to the system.

Each channel kind of its own factory address, each of these factory will
create the dedicated channel

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| _channelType | uint8 | Type of channel the factory is referring to `(standard=1 | multi=2)` |
| _scope | uint8 | Scope of the channel the factory is referring to `(public=1 | private=2)` |
| factoryAddr | address | Address of the factory contract |

### createChannel

```solidity
function createChannel(uint8 _channelType, uint8 _scope, string _name, string _tokenURI, bytes data) external payable
```

Create a new channel and process the initial configuration

`channelType` and `_scope` will define which kind of contract
we are targeting to create and `data` will just contains extra information needed for further
configuration of the channel

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| _channelType | uint8 | Type of the channel Possible values: - `1`: Standard channel - `2`: Multi-Channel |
| _scope | uint8 | Scope of the channel Possible values: - `1`: Public channel, anyone can mint onto the channel - `2`: Private channel, only owner of the channel can mint |
| _name | string | Name of the channel, will be set as name of the contract |
| _tokenURI | string |  |
| data | bytes | Extra data for configuration |

