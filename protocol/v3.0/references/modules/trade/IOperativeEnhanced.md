# IOperativeEnhanced
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/bc1f2ea3fd5d8b703627a7946e7d5fe7fb13f047/contracts/modules/trade/IOperativeEnhanced.sol)

**Inherits:**
[IOperative](/contracts/operative/IOperative.md), [IResellable](/contracts/modules/trade/IResellable.md)

**Title:**
IOperativeEnhanced

Provides methods and structs to manage operative enhanced flow.


## Functions
### owner


```solidity
function owner() external view returns (address);
```

### paymentProcessor


```solidity
function paymentProcessor() external view returns (IPaymentProcessor);
```

### approveOperatorForOwner

Records ERC-1155 approval of `operator` on behalf of the operative owner.
Callable only by the registered asset-creator delegate.


```solidity
function approveOperatorForOwner(address operator) external;
```

