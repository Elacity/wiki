## IPaymentProcessor

Defines how funds are executed between payer and recipient.

### execute

```solidity
function execute(address from, address to, uint256 _amount, address _payToken) external
```

Executes token-denominated payment routing.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| from | address | Payer address. |
| to | address | Recipient address. |
| _amount | uint256 | Amount to transfer. |
| _payToken | address | ERC-20 token used for payment. |

### execute

```solidity
function execute(address from, address to) external payable
```

Executes native-currency payment routing.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| from | address | Payer address. |
| to | address | Recipient address. |

### shouldTransferFund

```solidity
function shouldTransferFund() external view returns (bool)
```

Indicates whether this processor performs direct fund transfers.

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | bool | `true` when fund transfers are executed by the processor. |

## IDeferrablePayment

_This interface defines a payment processor that defer
payment statements by wrapping it with `defer` and `commit`
executions_

### OutOfCommitError

```solidity
error OutOfCommitError(address sender)
```

Thrown when `commit` is called without a matching deferred session.

### EmptyCommitError

```solidity
error EmptyCommitError()
```

Thrown when a deferred session has no pending payments to commit.

### PaymentCommitted

```solidity
event PaymentCommitted(address from, address recipient, address paymentToken, uint256 amount)
```

Emitted when a deferred payment is finalized.

### defer

```solidity
function defer(address from) external
```

Opens a deferred payment session for a payer.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| from | address | Address whose payments will be deferred. |

### commit

```solidity
function commit(address from, address _payToken) external payable
```

Commits and settles deferred payments for a payer.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| from | address | Address whose deferred queue is committed. |
| _payToken | address | Payment token used for settlement. |

### isDeferred

```solidity
function isDeferred(address from) external view returns (bool)
```

Returns whether an address currently has an active deferred session.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| from | address | Address to query. |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | bool | `true` if deferred mode is active. |

## IRewardsRecipient

Tracks and distributes accrued rewards by token.

### NoRewardsError

```solidity
error NoRewardsError(address beneficiary, address tokenAddress)
```

Thrown when no rewards are available for withdrawal.

### RewardsWithdrawn

```solidity
event RewardsWithdrawn(address to, uint256 amount, address paymentToken)
```

Emitted when rewards are withdrawn.

### RewardsIncremented

```solidity
event RewardsIncremented(address to, uint256 amount, address paymentToken, address _processor)
```

Emitted when rewards are credited to a recipient.

### rewardsOf

```solidity
function rewardsOf(address user, address paymentToken) external view returns (uint256)
```

Returns the rewards balance for a user and payment token.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| user | address | Reward beneficiary. |
| paymentToken | address | Token in which rewards are denominated. |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | uint256 | Accrued rewards amount. |

### withdrawRewards

```solidity
function withdrawRewards(address paymentToken) external
```

Withdraws caller rewards in the given token.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| paymentToken | address | Token to withdraw. |

### incrementRewards

```solidity
function incrementRewards(address to, uint256 _amount, address paymentToken) external
```

Credits rewards to a recipient.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| to | address | Reward beneficiary. |
| _amount | uint256 | Amount to credit. |
| paymentToken | address | Token denomination. |

## IConfigurablePayment

Allows governance/admin to set the active payment processor.

### setPaymentProcessor

```solidity
function setPaymentProcessor(address _payProc) external
```

Sets the payment processor contract.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| _payProc | address | Processor contract address. |

## IPaymentProcessorFactory

Creates payment processor instances bound to a recipient.

### createPaymentProcessor

```solidity
function createPaymentProcessor(address recipient, bool _shouldTransferFund) external returns (address)
```

Deploys a new payment processor.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| recipient | address | Default reward recipient. |
| _shouldTransferFund | bool | Whether processor should perform direct transfer. |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | address | Address of the deployed processor. |

