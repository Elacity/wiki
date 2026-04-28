# DigitalAssetCommon
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/52ca0e7824ef5fab5ebe0a131f7c6e6dd330de09/contracts/channel/kind/DigitalAssetCommon.sol)

**Inherits:**
Initializable, [StorageModule](/contracts/modules/core/StorageModule.md), [ChannelConfigurable](/contracts/channel/ChannelConfigurable.md), [SubscriptionModule](/contracts/modules/subscription/SubscriptionModule.md), [MintAssetFeeCollector](/contracts/modules/library/MintAssetFeeCollector.md)

**Title:**
DigitalAssetCommon

Shared base for public and private standard channels. Combines subscriptions,
royalty distribution, asset minting, and fee collection into a single upgradeable
ERC-1155 channel contract.

Subscription lookups propagate upward through the `ChannelRegistry` to resolve
multi-channel parent subscriptions.


## State Variables
### authority
Address of the AuthorityGateway that manages access-token trades and licensing.


```solidity
address public authority
```


### tradeGateway
Address of the TradeGateway that manages token-level marketplace trades.


```solidity
address public tradeGateway
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
    string memory _tokenUri,
    bytes memory _data
) public initializer;
```

### _createAsset

Mints a new unique digital asset on this channel.

The token ID is deterministically derived from `_tokenUri`. Reverts if the
generated ID is in the reserved range or has already been used.


```solidity
function _createAsset(string memory _tokenUri) internal returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_tokenUri`|`string`|IPFS-based URI pointing to the asset's JSON metadata|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The newly minted token ID|


### configureChannel

Applies the initial channel configuration during deployment.
Decodes `data` as `(ShareInput[], SubscriptionPlan[], TokenAccessThreshold[])` and:
1. Mints royalty-share tokens to each beneficiary.
2. Creates subscription plans on this channel.
3. Registers token-ownership-based access thresholds.

Example encoding with ethers.js:
```javascript
ethers.utils.defaultAbiCoder.encode(
['tuple(address,uint256)[]', 'tuple(uint8,address,uint256,uint256,bool)[]', 'tuple(address,uint256)[]'],
[
[['0x02...12e5', 800], ['0x52...D17B', 200]],   // royalty shares (beneficiary, share)
[[0, ethers.constants.AddressZero, 1e18, 2592000, true]], // subscription plans
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


### hasActiveSubscription

Returns whether an account currently has an active subscription.


```solidity
function hasActiveSubscription(address account) public view override returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`account`|`address`||

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|`true` if the account has a non-expired subscription.|


### _resolveEventHub

Resolves EventHub directly from the configured storage contract.


```solidity
function _resolveEventHub() internal view override returns (IEventHub);
```

### supportsInterface

Returns true if this contract implements the interface defined by
`interfaceId`. See the corresponding
https://eips.ethereum.org/EIPS/eip-165#how-interfaces-are-identified[ERC section]
to learn more about how these ids are created.
This function call must use less than 30 000 gas.


```solidity
function supportsInterface(bytes4 interfaceId) public view override(SubscriptionModule) returns (bool);
```

## Errors
### InvalidTokenId
The generated token ID is invalid or already minted.


```solidity
error InvalidTokenId();
```

