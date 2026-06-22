# UniversalPaymentTransferer
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/9e5d1dcd32c5761e2bd56d37138c1de7aac83865/contracts/modules/payment/UniversalPaymentTransferer.sol)

**Title:**
UniversalPaymentTransferer

Provides universal token transfer mechanisms for both ERC20 and native (ETH) tokens.

Helper library to abstract away token type differences during transfers.


## Functions
### transferFrom

Transfers the specified amount of tokens from the `from` address to the `to` address.

Security note (AV-8.1): payment flows must not assume `requested == received` for ERC-20.
Some tokens are fee-on-transfer/deflationary and credit the recipient less than the transfer amount.
If we continue after a short transfer, the protocol can over-credit internal rewards and become insolvent.


```solidity
function transferFrom(Amount memory _amount, address from, address to) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_amount`|`Amount`|The amount of tokens to transfer.|
|`from`|`address`|The address to transfer the tokens from.|
|`to`|`address`|The address to transfer the tokens to.|


## Errors
### PaymentAmountMismatch
Thrown when an ERC-20 transfer credits less than requested.


```solidity
error PaymentAmountMismatch(address token, uint256 expected, uint256 received);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`token`|`address`|ERC-20 token address.|
|`expected`|`uint256`|Requested transfer amount.|
|`received`|`uint256`|Actual recipient balance delta.|

