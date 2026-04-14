# IPaymentProcessor
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/a429f79c38ae4f5221da86eca62d9868f0a5a7fd/contracts/modules/payment/IPaymentProcessor.sol)

**Title:**
IPaymentProcessor

Defines how funds are executed between payer and recipient.


## Functions
### execute

Executes token-denominated payment routing.


```solidity
function execute(address from, address to, uint256 _amount, address _payToken) external;
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
function execute(address from, address to) external payable;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`from`|`address`|Payer address.|
|`to`|`address`|Recipient address.|


### shouldTransferFund

Indicates whether this processor performs direct fund transfers.


```solidity
function shouldTransferFund() external view returns (bool);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|`true` when fund transfers are executed by the processor.|


