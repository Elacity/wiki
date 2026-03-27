## MultiChannel

A bundle channel that wraps one or more standard channels. Subscribers to a
multi-channel automatically gain access to every wrapped (child) channel.

Unlike standard channels, a multi-channel does not host digital assets directly. It
holds its own royalty distribution and subscription plans, and registers itself as a
parent in the `ChannelRegistry` so that child channels can resolve access upward.

### name

```solidity
string name
```

### configureChannel

```solidity
function configureChannel(bytes data) internal
```

Applies the initial multi-channel configuration during deployment.

Decodes `data` as `(ShareInput[], SubscriptionPlan[], address[], TokenAccessThreshold[])`
and performs the following steps:
  1. Mints royalty-share tokens to each beneficiary.
  2. Creates subscription plans on this channel.
  3. Binds each child channel address into this multi-channel via the `ChannelRegistry`.
  4. Registers token-ownership-based access thresholds.

_Example encoding with ethers.js:
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
```_

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| data | bytes | ABI-encoded configuration payload |

### wrapChannel

```solidity
function wrapChannel(address addr) external
```

Register a new channel into a wrapper

_Restricted to channel admins to prevent unauthorized wrapper bindings._

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| addr | address | Channel to add |

### supportsInterface

```solidity
function supportsInterface(bytes4 interfaceId) public view returns (bool)
```

