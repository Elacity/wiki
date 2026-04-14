# ISubscriptionChannelHooks
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/a429f79c38ae4f5221da86eca62d9868f0a5a7fd/contracts/modules/subscription/SubscriptionManager.sol)


## Functions
### totalSupply

Returns the ERC-1155 total supply for a token id on the channel.


```solidity
function totalSupply(uint256 tokenId) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenId`|`uint256`|Token identifier.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Current total supply.|


### processSubscriptionState

Executes channel-side mint/payment effects for a successful subscription.


```solidity
function processSubscriptionState(
    address subscriber,
    ISubscriptionManageable.SubscriptionPlan calldata plan,
    uint256 subscriberCount,
    string calldata subscriptionTokenURI
) external payable;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`subscriber`|`address`|Subscriber account.|
|`plan`|`ISubscriptionManageable.SubscriptionPlan`|Selected plan.|
|`subscriberCount`|`uint256`|Existing subscriber count for plan before this call.|
|`subscriptionTokenURI`|`string`||


### setPlanTokenURI

Sets the token URI for a plan.


```solidity
function setPlanTokenURI(uint8 planId, string calldata planTokenURI) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`planId`|`uint8`|Plan identifier.|
|`planTokenURI`|`string`|Token URI.|


