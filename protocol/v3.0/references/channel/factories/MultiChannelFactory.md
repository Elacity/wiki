# MultiChannelFactory
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/9e5d1dcd32c5761e2bd56d37138c1de7aac83865/contracts/channel/factories/MultiChannelFactory.sol)

**Inherits:**
[IChannelFactory](/contracts/channel/factories/IChannelFactory.md), [ChannelFoundationFactory](/contracts/channel/factories/ChannelFoundationFactory.md), [StorageModule](/contracts/modules/core/StorageModule.md), [BeaconUpgradeableFactory](/contracts/modules/proxy/BeaconUpgradeableFactory.md), [MintChannelFeeCollector](/contracts/modules/library/MintChannelFeeCollector.md)

**Title:**
MultiChannelFactory

Factory that deploys `MultiChannel` contracts behind a beacon proxy.
Each deployed multi-channel receives its own payment processor and is
initialized with the provided configuration data (royalties, plans, child channels).


## State Variables
### authority

```solidity
address private immutable authority
```


### subscriptionManager

```solidity
address private immutable subscriptionManager
```


## Functions
### constructor

**Note:**
docs-ignore: true


```solidity
constructor(
    address _authority,
    address _tradeGateway,
    address _storeAddr,
    address _subscriptionManager,
    IPaymentProcessorFactory _ppf,
    address _implementation
) ChannelFoundationFactory(_ppf, _tradeGateway) BeaconUpgradeableFactory(_implementation, msg.sender);
```

### createChannel

Create a new channel contract
The kind of the target channel is defined in the factory implementation


```solidity
function createChannel(address creator, string memory _name, string memory _tokenURI, bytes calldata _data)
    external
    payable
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


### _configureChildWrappers

Binds the multi-channel's child channels after acknowledgement.

V3C-7 (CWE-665): wrapping is deferred out of `MultiChannel.initialize` because that runs inside
the BeaconProxy constructor — before `cstore.ack(channel)` and before the proxy has deployed code — so
an in-init `addWrapper` reverts `UnrecognizedContractError`. Replaying it here, after ack, lets the now
acknowledged channel register each binding via its admin-gated `wrapChannel` (the factory holds
`DEFAULT_ADMIN_ROLE`, granted to `msg.sender` during `initialize`).


```solidity
function _configureChildWrappers(address channel, bytes calldata data) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`channel`|`address`|Newly created multi-channel proxy.|
|`data`|`bytes`|ABI-encoded multi-channel creation config.|


### _configureTokenOwnershipAccess

Configures token-ownership access thresholds after channel acknowledgement.

Thresholds are decoded from multi-channel creation payload and applied through
`configureTokenOwnershipAccess` so EventHub emissions occur only after `cstore.ack(channel)`.


```solidity
function _configureTokenOwnershipAccess(address channel, bytes calldata data) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`channel`|`address`|Newly created multi-channel proxy.|
|`data`|`bytes`|ABI-encoded multi-channel creation config.|


## Structs
### ShareInput

```solidity
struct ShareInput {
    address beneficiary;
    uint256 share;
}
```

### SubscriptionPlan

```solidity
struct SubscriptionPlan {
    uint8 planId;
    address payToken;
    uint256 price;
    uint256 duration;
    bool active;
}
```

