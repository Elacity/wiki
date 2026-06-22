# RewardsRecipient
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/9e5d1dcd32c5761e2bd56d37138c1de7aac83865/contracts/modules/payment/RewardsRecipient.sol)

**Inherits:**
MulticallUpgradeable, [ConfigurablePaymentTrait](/contracts/modules/payment/ConfigurablePaymentTrait.md), [IRewardsRecipient](/contracts/modules/payment/IRewardsRecipient.md), [WithdrawReentrancyGuard](/contracts/modules/payment/WithdrawReentrancyGuard.md)

**Title:**
RewardsRecipient

Allows receiving, accumulating, and withdrawing rewards for different addresses.

Integrates with `ConfigurablePaymentTrait` to ensure only the configured processor can increment rewards.


## State Variables
### REWARDS_RECIPIENT_STORAGE_LOCATION

```solidity
bytes32 private constant REWARDS_RECIPIENT_STORAGE_LOCATION =
    0x1d1f07d282138f199d2bac7f37e86c96e6ef001fc027770bb13d2db59af86400
```


## Functions
### _getRewardsRecipientStorage


```solidity
function _getRewardsRecipientStorage() private pure returns (RewardsRecipientStorage storage $);
```

### rewardsOf

Get the rewards for an address.


```solidity
function rewardsOf(address user, address paymentToken) public view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`user`|`address`|The address to get the rewards for.|
|`paymentToken`|`address`|The payment token to get the rewards for.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The rewards for the address.|


### pendingRefunds

Get the pending refunds for an address.


```solidity
function pendingRefunds(address account) public view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`account`|`address`|The address to get the pending refunds for.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The pending refunds for the address.|


### onlyProcessor

Modifier to ensure the caller is a recognized processor.


```solidity
modifier onlyProcessor() ;
```

### _onlyProcessor


```solidity
function _onlyProcessor() internal view;
```

### __RewardsRecipient_init

**Note:**
docs-ignore: true


```solidity
function __RewardsRecipient_init() internal onlyInitializing;
```

### _afterProcessorSet


```solidity
function _afterProcessorSet(address _payProc) internal override;
```

### _creditPendingRefund


```solidity
function _creditPendingRefund(address account, uint256 amount) internal;
```

### incrementRewards

Increments the rewards for an address.


```solidity
function incrementRewards(address to, uint256 _amount, address _paymentToken) external onlyProcessor;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`to`|`address`|The address to increment the reward for.|
|`_amount`|`uint256`|The amount of reward to increment.|
|`_paymentToken`|`address`|The payment token.|


### withdrawRewards

Withdraws the rewards for an address.


```solidity
function withdrawRewards(address _paymentToken) external noReentrantWithdraw;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_paymentToken`|`address`|The payment token.|


### claimRefund

Claims a pending native-currency refund.


```solidity
function claimRefund() external noReentrantWithdraw;
```

## Errors
### UnauthorizedError
Thrown when a non-recognized payment processor tries to mutate rewards.


```solidity
error UnauthorizedError(address _proc);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_proc`|`address`|Caller address that failed processor authorization.|

## Structs
### RewardsRecipientStorage
**Note:**
storage-location: erc7201:elacity.drm.storage.RewardsRecipient


```solidity
struct RewardsRecipientStorage {
    mapping(address => mapping(address => uint256)) rewardsOf;
    mapping(address => uint256) pendingRefunds;
    EnumerableSet.AddressSet recognizedProcessor;
}
```

