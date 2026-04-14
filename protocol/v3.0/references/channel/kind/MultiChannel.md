# MultiChannel
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/a429f79c38ae4f5221da86eca62d9868f0a5a7fd/contracts/channel/kind/MultiChannel.sol)

**Inherits:**
Initializable, [StorageModule](/contracts/modules/core/StorageModule.md), [ProtocolVersioned](/contracts/library/ProtocolVersioned.md), [ChannelConfigurable](/contracts/channel/ChannelConfigurable.md), [SubscriptionModule](/contracts/modules/subscription/SubscriptionModule.md), [IChannelWrapper](/contracts/channel/IChannelWrapper.md)

**Title:**
MultiChannel

A bundle channel that wraps one or more standard channels. Subscribers to a
multi-channel automatically gain access to every wrapped (child) channel.
Unlike standard channels, a multi-channel does not host digital assets directly. It
holds its own royalty distribution and subscription plans, and registers itself as a
parent in the `ChannelRegistry` so that child channels can resolve access upward.


## State Variables
### authority
Address of the AuthorityGateway that manages access-token trades and licensing.


```solidity
address private authority
```


### tradeGateway
Address of the TradeGateway that manages token-level marketplace trades.


```solidity
address public tradeGateway
```


### name
Human-readable name of this channel.


```solidity
string public name
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

**Note:**
docs-ignore: true


```solidity
function initialize(
    address _authority,
    address _tradeGateway,
    address _registry,
    address _subscriptionManager,
    address creator,
    string memory _name,
    string memory _tokenURI,
    bytes memory _data
) public initializer;
```

### configureChannel

Applies the initial multi-channel configuration during deployment.
Decodes `data` as `(ShareInput[], SubscriptionPlan[], address[], TokenAccessThreshold[])`
and performs the following steps:
1. Mints royalty-share tokens to each beneficiary.
2. Creates subscription plans on this channel.
3. Binds each child channel address into this multi-channel via the `ChannelRegistry`.
4. Registers token-ownership-based access thresholds.

Example encoding with ethers.js:
```javascript
ethers.utils.defaultAbiCoder.encode(
['tuple(address,uint256)[]', 'tuple(uint8,address,uint256,uint256,bool)[]', 'address[]', 'tuple(address,uint256)[]'],
[
[['0x02...12e5', 800], ['0x52...D17B', 200]],   // royalty shares
[[0, ethers.constants.AddressZero, 1e18, 2592000, true]], // subscription plans
['0x09...C52D', '0x14...6fdf'],                  // child channel addresses to wrap
[]  // token-ownership thresholds
]
);
```


```solidity
function configureChannel(bytes memory data) internal override;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`data`|`bytes`|ABI-encoded configuration payload|


### wrapChannel

Register a new channel into a wrapper

Restricted to channel admins to prevent unauthorized wrapper bindings.


```solidity
function wrapChannel(address addr) external onlyRole(DEFAULT_ADMIN_ROLE);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`addr`|`address`|Channel to add|


### _resolveEventHub

Resolves EventHub directly from the configured storage contract.


```solidity
function _resolveEventHub() internal view override returns (IEventHub);
```

### supportsInterface

Resolves the diamond-inheritance `supportsInterface` conflict.


```solidity
function supportsInterface(bytes4 interfaceId) public view override(SubscriptionModule) returns (bool);
```

