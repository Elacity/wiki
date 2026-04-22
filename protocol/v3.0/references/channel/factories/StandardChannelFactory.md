# StandardChannelFactory
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/52ca0e7824ef5fab5ebe0a131f7c6e6dd330de09/contracts/channel/factories/StandardChannelFactory.sol)

**Inherits:**
[IChannelFactory](/contracts/channel/factories/IChannelFactory.md), [ChannelFoundationFactory](/contracts/channel/factories/ChannelFoundationFactory.md), [StorageModule](/contracts/modules/core/StorageModule.md), [BeaconUpgradeableFactory](/contracts/modules/proxy/BeaconUpgradeableFactory.md), [MintChannelFeeCollector](/contracts/modules/library/MintChannelFeeCollector.md)

**Title:**
StandardChannelFactory

Abstract factory for deploying standard (asset-hosting) channels behind a beacon proxy.
Concrete subclasses (`StandardChannelPublicFactory`, `StandardChannelPrivateFactory`)
select the appropriate channel implementation at deployment time.


## State Variables
### authority

```solidity
address internal immutable authority
```


### subscriptionManager

```solidity
address internal immutable subscriptionManager
```


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
) ChannelFoundationFactory(_ppf, _tradeGateway) BeaconUpgradeableFactory(_implementation, msg.sender);
```

### createChannel

Create a new channel contract
The kind of the target channel is defined in the factory implementation


```solidity
function createChannel(address creator, string memory _name, string memory _tokenUri, bytes calldata _data)
    external
    payable
    virtual
    returns (address);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`creator`|`address`|Address of the creator/owner, basically it's the initiator of the transaction|
|`_name`|`string`|The name of the channel|
|`_tokenUri`|`string`|Base metadata URI for the channel contract|
|`_data`|`bytes`|Initialization and configuration data|


### _configureTokenOwnershipAccess

Configures token-ownership access thresholds after channel acknowledgement.

Thresholds are decoded from channel creation payload and applied through
`configureTokenOwnershipAccess` so EventHub emissions occur only after `cstore.ack(channel)`.


```solidity
function _configureTokenOwnershipAccess(address channel, bytes calldata data) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`channel`|`address`|Newly created channel proxy.|
|`data`|`bytes`|ABI-encoded standard channel creation config.|


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

