# ChannelFactory
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/a429f79c38ae4f5221da86eca62d9868f0a5a7fd/contracts/channel/ChannelFactory.sol)

**Inherits:**
Initializable, OwnableUpgradeable

**Title:**
ChannelFactory

Channel creation router that mirrors the legacy `ChannelCore` flow.
It routes channel creation requests to a registered concrete factory based on
`(channelType, scope)`.


## State Variables
### factories
Mapping: channel type => scope => factory address.


```solidity
mapping(uint8 => mapping(uint8 => address)) public factories
```


## Functions
### constructor

**Notes:**
- oz-upgrades-unsafe-allow: constructor

- docs-ignore: true


```solidity
constructor() ;
```

### initialize

Initializes the contract and sets the owner.

Designed for proxy deployments, matching the legacy ChannelCore pattern.


```solidity
function initialize() external initializer;
```

### registerFactory

Registers or updates the factory address for a `(channelType, scope)` pair.


```solidity
function registerFactory(uint8 _channelType, uint8 _scope, address factoryAddr) external onlyOwner;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_channelType`|`uint8`|Channel kind (for example standard or multi).|
|`_scope`|`uint8`|Visibility scope (for example public/private).|
|`factoryAddr`|`address`|Address of the concrete factory implementing `IChannelFactory`.|


### createChannel

Creates a channel by forwarding the call to the registered concrete factory.


```solidity
function createChannel(
    uint8 _channelType,
    uint8 _scope,
    string memory _name,
    string memory _tokenURI,
    bytes memory data
) external payable returns (address);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_channelType`|`uint8`|Channel kind (for example standard or multi).|
|`_scope`|`uint8`|Visibility scope (for example public/private).|
|`_name`|`string`|Human-readable channel name.|
|`_tokenURI`|`string`|Base metadata URI for the channel contract.|
|`data`|`bytes`|Channel initialization/configuration payload.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|Address of the deployed channel proxy.|


## Events
### FactoryRegistered
Emitted when a factory is registered or updated.


```solidity
event FactoryRegistered(uint8 indexed channelType, uint8 indexed scope, address indexed factoryAddr);
```

### ChannelCreated
Emitted when a new channel is created through a registered factory.


```solidity
event ChannelCreated(
    uint8 indexed channelType, uint8 indexed scope, address indexed creator, address channel, address factoryAddr
);
```

## Errors
### InvalidFactoryAddress
Thrown when trying to register a zero-address factory.


```solidity
error InvalidFactoryAddress(address factoryAddr);
```

### FactoryNotRegistered
Thrown when no factory is registered for a `(channelType, scope)` pair.


```solidity
error FactoryNotRegistered(uint8 channelType, uint8 scope);
```

