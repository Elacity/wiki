# WithdrawablePaymentProcessor
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/674fb60a18e2aa14b7080f0f43e11002723bd5b3/contracts/modules/payment/WithdrawablePaymentProcessor.sol)

**Inherits:**
IERC165, Initializable, ContextUpgradeable, OwnableUpgradeable, ReentrancyGuard, [IPaymentProcessor](/contracts/modules/payment/IPaymentProcessor.md), [IDeferrablePayment](/contracts/modules/payment/IDeferrablePayment.md)

**Title:**
WithdrawablePaymentProcessor

Routes payments into reward balances and optionally settles deferred transfers in bulk.

Deployed behind beacon proxies via `PaymentProcessorFactory`.


## State Variables
### shouldTransferFund
Indicates whether this processor performs direct fund transfers.


```solidity
bool public shouldTransferFund
```


### recipient
Rewards recipient contract.


```solidity
IRewardsRecipient public recipient
```


### WITHDRAWABLE_PROCESSOR_STORAGE_SLOT

```solidity
bytes32 private constant WITHDRAWABLE_PROCESSOR_STORAGE_SLOT =
    0xd454f75424acd3d14d0b26430b4bd3aeea9f44546eb418fdc8ab691cfa28a500
```


## Functions
### isContract


```solidity
modifier isContract() ;
```

### _getWithdrawableProcessorStorage


```solidity
function _getWithdrawableProcessorStorage() internal pure returns (PaymentProcessorStorage storage $);
```

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
function initialize(address _recipient, bool _transferFund) public initializer;
```

### _checkWhetherContract


```solidity
function _checkWhetherContract() internal view;
```

### execute

Executes token-denominated payment routing.


```solidity
function execute(address from, address to, uint256 _amount, address _payToken) external isContract nonReentrant;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`from`|`address`|Payer address.|
|`to`|`address`|Recipient address.|
|`_amount`|`uint256`|Amount to transfer.|
|`_payToken`|`address`|ERC-20 token used for payment.|


### execute

Executes native-currency payment routing.


```solidity
function execute(address from, address to) external payable isContract nonReentrant;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`from`|`address`|Payer address.|
|`to`|`address`|Recipient address.|


### defer

Opens a deferred payment session for a payer.


```solidity
function defer(address from) public isContract nonReentrant;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`from`|`address`|Address whose payments will be deferred.|


### commit

Commits and settles deferred payments for a payer.


```solidity
function commit(address from, address _payToken) external payable isContract nonReentrant;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`from`|`address`|Address whose deferred queue is committed.|
|`_payToken`|`address`|Payment token used for settlement.|


### supportsInterface

Returns true if this contract implements the interface defined by
`interfaceId`. See the corresponding
https://eips.ethereum.org/EIPS/eip-165#how-interfaces-are-identified[ERC section]
to learn more about how these ids are created.
This function call must use less than 30 000 gas.


```solidity
function supportsInterface(bytes4 interfaceId) external pure returns (bool);
```

### _lockPayment


```solidity
function _lockPayment(Amount memory am, address from, address to) private;
```

### isDeferred

Returns whether an address currently has an active deferred session.


```solidity
function isDeferred(address from) public view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`from`|`address`|Address to query.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|`true` if deferred mode is active.|


### receive

**Note:**
docs-ignore: true


```solidity
receive() external payable;
```

## Events
### AtomicNativeTransfer
Emitted for native-value atomic path.


```solidity
event AtomicNativeTransfer(address indexed from, address indexed to, uint256 amount);
```

## Errors
### NotContractError
Thrown when an EOA directly calls deferred/processor methods.


```solidity
error NotContractError(address caller);
```

### InvalidRecipient
Thrown when recipient address is zero.


```solidity
error InvalidRecipient(address recipientAddress);
```

## Structs
### PaymentProcessorStorage
Storage layout for `WithdrawablePaymentProcessor`.

This struct is used to manage the state of the contract.

**Note:**
storage-location: erc7201:elacity.drm.storage.WithdrawablePaymentProcessor


```solidity
struct PaymentProcessorStorage {
    /// @notice Deferred payment accumulation: payer => token => amount.
    mapping(address user => mapping(address paymentToken => uint256 amount)) committablePayment;
    /// @notice Whether the payment is deferred.
    mapping(address user => bool) isDeferred;
    /// @notice Contract that opened deferred payment session per payer.
    mapping(address user => address caller) deferredSessionOwner;
}
```

