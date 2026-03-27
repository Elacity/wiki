# ISubscriptionChannelHooks
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/674fb60a18e2aa14b7080f0f43e11002723bd5b3/contracts/modules/subscription/SubscriptionManager.sol)


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


