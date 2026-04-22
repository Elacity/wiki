# TradeRestrictionExtension
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/52ca0e7824ef5fab5ebe0a131f7c6e6dd330de09/contracts/modules/trade/TradeRestrictionExtension.sol)

**Inherits:**
Initializable, ContextUpgradeable

**Title:**
TradeRestrictionExtension

Enforces trade-access restrictions on token operations by checking whether the
target contract supports `ITradeAccessRestriction` and whether the caller is permitted to
trade the specified token.


## Functions
### __TradeRestrictionExtension_init

**Note:**
docs-ignore: true


```solidity
function __TradeRestrictionExtension_init() internal onlyInitializing;
```

### restrictTradeOf

Guards a function so that only callers with trade access on the target
contract and token may proceed.


```solidity
modifier restrictTradeOf(address contractAddress, uint256 tkId) ;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`contractAddress`|`address`|Address of the ERC-1155 contract to check.|
|`tkId`|`uint256`|Token ID being traded.|


### _restrictTradeOf

Checks if the caller has trade access on the target contract and token.


```solidity
function _restrictTradeOf(address contractAddress, uint256 tkId) internal view;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`contractAddress`|`address`|Address of the ERC-1155 contract to check.|
|`tkId`|`uint256`|Token ID being traded.|


## Errors
### TradableContractFault
The target contract does not implement `ITradeAccessRestriction` via ERC-165.


```solidity
error TradableContractFault(address contractAddress);
```

### TradeActionRestricted
The caller is not permitted to trade the specified token.


```solidity
error TradeActionRestricted(address account);
```

