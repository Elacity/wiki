## Amount

```solidity
struct Amount {
  uint256 value;
  address payToken;
}
```

## UniversalPaymentTransferer

Provides universal token transfer mechanisms for both ERC20 and native (ETH) tokens.

_Helper library to abstract away token type differences during transfers._

### PaymentAmountMismatch

```solidity
error PaymentAmountMismatch(address token, uint256 expected, uint256 received)
```

Thrown when an ERC-20 transfer credits less than requested.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| token | address | ERC-20 token address. |
| expected | uint256 | Requested transfer amount. |
| received | uint256 | Actual recipient balance delta. |

### transferFrom

```solidity
function transferFrom(struct Amount _amount, address from, address to) external
```

Transfers the specified amount of tokens from the `from` address to the `to` address.

_Security note (AV-8.1): payment flows must not assume `requested == received` for ERC-20.
Some tokens are fee-on-transfer/deflationary and credit the recipient less than the transfer amount.
If we continue after a short transfer, the protocol can over-credit internal rewards and become insolvent._

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| _amount | struct Amount | The amount of tokens to transfer. |
| from | address | The address to transfer the tokens from. |
| to | address | The address to transfer the tokens to. |

## ConfigurablePaymentTrait

Exposes an extendable interface to configure a customized payment processor.

_Allows inheriting contracts to integrate with different `IPaymentProcessor` implementations._

### paymentProcessor

```solidity
contract IPaymentProcessor paymentProcessor
```

Address of the implementation of the payment processor

### _setPaymentProcessor

```solidity
function _setPaymentProcessor(address _payProc) internal
```

Sets the payment processor.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| _payProc | address | The address of the payment processor. |

### setPaymentProcessor

```solidity
function setPaymentProcessor(address _payProc) external virtual
```

Sets the payment processor.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| _payProc | address | The address of the payment processor. |

### _afterProcessorSet

```solidity
function _afterProcessorSet(address _payProc) internal virtual
```

Called after the payment processor is set.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| _payProc | address | The address of the payment processor. |

## RewardsRecipient

Allows receiving, accumulating, and withdrawing rewards for different addresses.

_Integrates with `ConfigurablePaymentTrait` to ensure only the configured processor can increment rewards._

### UnauthorizedError

```solidity
error UnauthorizedError(address _proc)
```

Thrown when a non-recognized payment processor tries to mutate rewards.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| _proc | address | Caller address that failed processor authorization. |

### RewardsReentrantCall

```solidity
error RewardsReentrantCall()
```

Thrown when a reentrant rewards withdrawal is attempted.

### rewardsOf

```solidity
mapping(address => mapping(address => uint256)) rewardsOf
```

Returns the rewards balance for a user and payment token.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |

### noReentrantWithdraw

```solidity
modifier noReentrantWithdraw()
```

Modifier to prevent reentrancy in withdrawRewards.

### onlyProcessor

```solidity
modifier onlyProcessor()
```

Modifier to ensure the caller is a recognized processor.

### _afterProcessorSet

```solidity
function _afterProcessorSet(address _payProc) internal
```

Called after the payment processor is set.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| _payProc | address | The address of the payment processor. |

### incrementRewards

```solidity
function incrementRewards(address to, uint256 _amount, address _paymentToken) external
```

Increments the rewards for an address.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| to | address | The address to increment the reward for. |
| _amount | uint256 | The amount of reward to increment. |
| _paymentToken | address | The payment token. |

### withdrawRewards

```solidity
function withdrawRewards(address _paymentToken) external
```

Withdraws the rewards for an address.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| _paymentToken | address | The payment token. |

## WithdrawablePaymentProcessor

Process payments by incrementing rewards for users instead of direct transfers, or deferring them for bulk payouts.

_Works alongside `RewardsRecipient` to securely manage deferrable and atomic payment splits._

### NotContractError

```solidity
error NotContractError(address caller)
```

Thrown when an EOA directly calls deferred/processor methods.

### shouldTransferFund

```solidity
bool shouldTransferFund
```

Whether the payment processor should transfer funds directly.

### recipient

```solidity
contract IRewardsRecipient recipient
```

Address of the rewards recipient.

### isDeferred

```solidity
mapping(address => bool) isDeferred
```

Whether the payment is deferred.

### AtomicNativeTransfer

```solidity
event AtomicNativeTransfer(address from, address to, uint256 amount, address payToken)
```

Emitted when a payment is processed atomically.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| from | address | The address to transfer the payment from. |
| to | address | The address to transfer the payment to. |
| amount | uint256 | The amount of payment to transfer. |
| payToken | address | The payment token. |

### isContract

```solidity
modifier isContract()
```

### execute

```solidity
function execute(address from, address to, uint256 _amount, address _payToken) external
```

Executes a payment.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| from | address | The address to transfer the payment from. |
| to | address | The address to transfer the payment to. |
| _amount | uint256 | The amount of payment to transfer. |
| _payToken | address | The payment token. |

### execute

```solidity
function execute(address from, address to) external payable
```

Executes a payment with native currency.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| from | address | The address to transfer the payment from. |
| to | address | The address to transfer the payment to. |

### defer

```solidity
function defer(address from) public
```

Defers the payment for an address.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| from | address | The address to defer the payment for. |

### commit

```solidity
function commit(address from, address _payToken) external payable
```

Commits the payment for an address.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| from | address | The address to commit the payment for. |
| _payToken | address | The payment token. |

### supportsInterface

```solidity
function supportsInterface(bytes4 interfaceId) external view virtual returns (bool)
```

_Returns true if this contract implements the interface defined by
`interfaceId`. See the corresponding
https://eips.ethereum.org/EIPS/eip-165#how-interfaces-are-identified[ERC section]
to learn more about how these ids are created.

This function call must use less than 30 000 gas._

## PaymentProcessorFactory

Factory to deploy new instances of `WithdrawablePaymentProcessor` clones as beacon proxies.

_Uses OpenZeppelin's Beacon proxy pattern for upgradeable clones._

### constructor

```solidity
constructor(address _implementation) public
```

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

## PaymentModule

Abstract module dealing with payment routing, deferment, and platform fee deductions.

_Core payment logic to be inherited by modules requiring value transfers and splits._

### PaymentLog

```solidity
event PaymentLog(address from, address to, uint256 amount, address paymentToken)
```

Event triggered on each payment processed regardless of how it is processed

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| from | address | sender of the payment |
| to | address | recipient of the payment |
| amount | uint256 | the amount in `wei` to pay |
| paymentToken | address | address of the payment method |

### deferPaymentIfQualified

```solidity
modifier deferPaymentIfQualified(contract IPaymentProcessor payer, address from, struct IERC2981Enhanced.RoyaltyInfo[] rs, address _payToken)
```

### __PaymentModule_init

```solidity
function __PaymentModule_init() internal
```

### receive

```solidity
receive() external payable virtual
```

### _payAmount

```solidity
function _payAmount(contract IPaymentProcessor paymentProcessor, address from, address to, uint256 _amount, address _payToken) internal
```

main entrypoint of the payment processing

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| paymentProcessor | contract IPaymentProcessor |  |
| from | address |  |
| to | address | recipient of the payment |
| _amount | uint256 | the amount in `wei` to pay |
| _payToken | address | address of the payment method |

### _deductPlatformFee

```solidity
function _deductPlatformFee(contract IStorage store, address from, uint256 _amount, address _payToken) internal returns (uint256 amount)
```

