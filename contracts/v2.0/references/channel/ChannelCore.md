## ChannelCore

Orchestrates the creation of content channels by delegating to registered factory contracts.
Each combination of channel type and scope maps to a dedicated factory that deploys the appropriate
channel implementation behind a beacon proxy.

_Upgradeable; owner-only factory registration._

### CreateFailed

```solidity
error CreateFailed()
```

The factory returned `address(0)` for the newly created channel.

### FactoryUnknownError

```solidity
error FactoryUnknownError(bytes rawError)
```

The factory call reverted with an unexpected error.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| rawError | bytes | The raw revert bytes forwarded from the factory |

### FactoryNotFound

```solidity
error FactoryNotFound(uint8 _channelType, uint8 _scope)
```

No factory is registered for the requested channel type and scope combination.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| _channelType | uint8 | The requested channel type |
| _scope | uint8 | The requested scope |

### ChannelCreated

```solidity
event ChannelCreated(address channelAddr, uint8 channelType, address owner, uint8 scope, string name)
```

Emitted when a new channel contract is successfully deployed.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| channelAddr | address | Address of the newly created channel proxy |
| channelType | uint8 | Type identifier (`1` = Standard, `2` = Multi-Channel) |
| owner | address | Address of the channel creator |
| scope | uint8 | Visibility scope (`1` = Public, `2` = Private) |
| name | string | Human-readable name assigned to the channel |

### registerFactory

```solidity
function registerFactory(uint8 _channelType, uint8 _scope, address factoryAddr) external
```

Registers (or replaces) the factory contract for a given channel type and scope.

_Only the contract owner may call this function._

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| _channelType | uint8 | Channel type the factory handles (`1` = Standard, `2` = Multi-Channel) |
| _scope | uint8 | Scope the factory handles (`1` = Public, `2` = Private) |
| factoryAddr | address | Address of the `IChannelFactory` implementation |

### createChannel

```solidity
function createChannel(uint8 _channelType, uint8 _scope, string _name, string _tokenURI, bytes data) external payable
```

Deploys a new channel contract and applies its initial configuration.

The `_channelType` and `_scope` pair selects the factory, while `data` carries
ABI-encoded parameters for royalty shares, subscription plans, and other settings
specific to the chosen channel kind.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| _channelType | uint8 | Type of channel to create:   - `1` — Standard channel (hosts digital assets directly)   - `2` — Multi-Channel (wraps other channels for bundled subscriptions) |
| _scope | uint8 | Visibility scope of the channel:   - `1` — Public (any user can mint content)   - `2` — Private (only the channel owner can mint) |
| _name | string | Human-readable name for the channel (stored as the contract `name`) |
| _tokenURI | string | Base metadata URI for the channel's tokens |
| data | bytes | ABI-encoded initialization payload forwarded to the factory |

