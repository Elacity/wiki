# IRewardsRecipient
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/52ca0e7824ef5fab5ebe0a131f7c6e6dd330de09/contracts/modules/payment/IRewardsRecipient.sol)

**Title:**
IRewardsRecipient

Tracks and distributes accrued rewards by token.


## Functions
### rewardsOf

Returns the rewards balance for a user and payment token.


```solidity
function rewardsOf(address user, address paymentToken) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|Reward beneficiary.|
|`paymentToken`|`address`|Token in which rewards are denominated.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Accrued rewards amount.|


### withdrawRewards

Withdraws caller rewards in the given token.


```solidity
function withdrawRewards(address paymentToken) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`paymentToken`|`address`|Token to withdraw.|


### pendingRefunds

Returns the pending native-currency refund for an account.


```solidity
function pendingRefunds(address account) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`account`|`address`|The account to query.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Pending refund amount.|


### claimRefund

Claims a pending native-currency refund.


```solidity
function claimRefund() external;
```

### incrementRewards

Credits rewards to a recipient.


```solidity
function incrementRewards(address to, uint256 _amount, address paymentToken) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`to`|`address`|Reward beneficiary.|
|`_amount`|`uint256`|Amount to credit.|
|`paymentToken`|`address`|Token denomination.|


## Events
### RewardsWithdrawn
Emitted when rewards are withdrawn.


```solidity
event RewardsWithdrawn(address indexed to, uint256 amount, address indexed paymentToken);
```

### RewardsIncremented
Emitted when rewards are credited to a recipient.


```solidity
event RewardsIncremented(
    address indexed to, uint256 amount, address indexed paymentToken, address indexed _processor
);
```

### RefundCredited
Emitted when a native-currency refund is credited for later claim.


```solidity
event RefundCredited(address indexed account, uint256 amount);
```

### RefundClaimed
Emitted when a credited refund is claimed.


```solidity
event RefundClaimed(address indexed account, uint256 amount);
```

## Errors
### NoRewardsError
Thrown when no rewards are available for withdrawal.


```solidity
error NoRewardsError(address beneficiary, address tokenAddress);
```

### NoRefundError
Thrown when no pending refund is available for claim.


```solidity
error NoRefundError(address account);
```

