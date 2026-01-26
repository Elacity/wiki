## Amount

```solidity
struct Amount {
  uint256 value;
  address payToken;
}
```

## UniversalPaymentTransferer

### transferFrom

```solidity
function transferFrom(struct Amount _amount, address from, address to) external
```

## ConfigurablePaymentTrait

### paymentProcessor

```solidity
contract IPaymentProcessor paymentProcessor
```

Address of the implementation of the payment processor

### _setPaymentProcessor

```solidity
function _setPaymentProcessor(address _payProc) internal
```

### setPaymentProcessor

```solidity
function setPaymentProcessor(address _payProc) external
```

### _afterProcessorSet

```solidity
function _afterProcessorSet(address _payProc) internal virtual
```

## RewardsRecipient

### UnauthorizedError

```solidity
error UnauthorizedError(address _proc)
```

### rewardsOf

```solidity
mapping(address => mapping(address => uint256)) rewardsOf
```

### onlyProcessor

```solidity
modifier onlyProcessor()
```

### __RewardsRecipient_init

```solidity
function __RewardsRecipient_init() internal
```

### _afterProcessorSet

```solidity
function _afterProcessorSet(address _payProc) internal
```

### incrementRewards

```solidity
function incrementRewards(address to, uint256 _amount, address _paymentToken) external
```

### withdrawRewards

```solidity
function withdrawRewards(address _paymentToken) external
```

## WithdrawablePaymentProcessor

### shouldTransferFund

```solidity
bool shouldTransferFund
```

### recipient

```solidity
contract IRewardsRecipient recipient
```

### isDeferred

```solidity
mapping(address => bool) isDeferred
```

### AtomicNativeTransfer

```solidity
event AtomicNativeTransfer(address from, address to, uint256 amount, address payToken)
```

### constructor

```solidity
constructor() public
```

### initialize

```solidity
function initialize(address _recipient, bool _transferFund) public
```

### execute

```solidity
function execute(address from, address to, uint256 _amount, address _payToken) external
```

### execute

```solidity
function execute(address from, address to) external payable
```

### defer

```solidity
function defer(address from) public
```

### commit

```solidity
function commit(address from, address _payToken) external payable
```

### supportsInterface

```solidity
function supportsInterface(bytes4 interfaceId) external view virtual returns (bool)
```

_Returns true if this contract implements the interface defined by
`interfaceId`. See the corresponding
https://eips.ethereum.org/EIPS/eip-165#how-interfaces-are-identified[ERC section]
to learn more about how these ids are created.

This function call must use less than 30 000 gas._

### receive

```solidity
receive() external payable virtual
```

## PaymentProcessorFactory

### constructor

```solidity
constructor(address _implementation) public
```

### createPaymentProcessor

```solidity
function createPaymentProcessor(address recipient, bool _shouldTransferFund) external returns (address)
```

## PaymentModule

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

