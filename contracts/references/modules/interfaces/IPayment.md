## IPaymentProcessor

### execute

```solidity
function execute(address from, address to, uint256 _amount, address _payToken) external
```

### execute

```solidity
function execute(address from, address to) external payable
```

### shouldTransferFund

```solidity
function shouldTransferFund() external view returns (bool)
```

## IDeferrablePayment

_This interface defines a payment processor that defer
payment statements by wrapping it with `defer` and `commit`
executions_

### OutOfCommitError

```solidity
error OutOfCommitError(address sender)
```

### EmptyCommitError

```solidity
error EmptyCommitError()
```

### PaymentCommitted

```solidity
event PaymentCommitted(address from, address recipient, address paymentToken, uint256 amount)
```

### defer

```solidity
function defer(address from) external
```

### commit

```solidity
function commit(address from, address _payToken) external payable
```

### isDeferred

```solidity
function isDeferred(address from) external view returns (bool)
```

## IRewardsRecipient

### NoRewardsError

```solidity
error NoRewardsError(address beneficiary, address tokenAddress)
```

### RewardsWithdrawn

```solidity
event RewardsWithdrawn(address to, uint256 amount, address paymentToken)
```

### RewardsIncremented

```solidity
event RewardsIncremented(address to, uint256 amount, address paymentToken, address _processor)
```

### rewardsOf

```solidity
function rewardsOf(address user, address paymentToken) external view returns (uint256)
```

### withdrawRewards

```solidity
function withdrawRewards(address paymentToken) external
```

### incrementRewards

```solidity
function incrementRewards(address to, uint256 _amount, address paymentToken) external
```

## IConfigurablePayment

### setPaymentProcessor

```solidity
function setPaymentProcessor(address _payProc) external
```

## IPaymentProcessorFactory

### createPaymentProcessor

```solidity
function createPaymentProcessor(address recipient, bool _shouldTransferFund) external returns (address)
```

